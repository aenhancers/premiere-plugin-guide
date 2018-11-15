.. _exporters/selector-descriptions:

Selector Descriptions
################################################################################

This section provides a brief overview of each selector and highlights implementation issues.

Additional implementation details are at the end of the chapter.

----

exSelStartup
================================================================================

- param1 - ``exExporterInfoRec*``
- param2 - ``unused``

Sent during application launch, unless the exporter has been cached.

A single exporter can support multiple codecs and file extensions.

``exExporterInfoRec`` describes the exporter's attributes, such as the format display name.

----

exSelBeginInstance
================================================================================

- param1 - ``exExporterInstanceRec*``
- param2 - ``unused``

Allocate any private data.

----

exSelGenerateDefaultParams
================================================================================

- param1 - ``exGenerateDefaultParamRec*``
- param2 - ``unused``

Set the exporter's default parameters using the Export Param Suite.

----

exSelPostProcessParams
================================================================================

- param1 - ``exPostProcessParamsRec*``
- param2 - ``unused``

Post process parameters. This is where the localized strings for the parameter UI must be provided.

----

exSelValidateParamChanged
================================================================================

- param1 - ``exParamChangedRec*``
- param2 - ``unused``

Validate any parameters that have changed. Based on a change to a parameter value, the exporter may update other parameter values, or show/hide certain parameter controls, using the Export Param Suite.

To notify the host that the plug-in is changing other parameters, set exParam­ ``ChangedRec.rebuildAllParams`` to a non-zero value.

----

exSelGetParamSummary
================================================================================

- param1 - ``exParamSummaryRec*``
- param2 - ``unused``

Provide a text summary of the current parameter settings, which will be displayed in the sum- mary area of the Export Settings dialog.

----

exSelParamButton
================================================================================

- param1 - ``exParamButtonRec*``
- param2 - ``unused``

Sent if exporter has one or more buttons in its parameter UI, and the user clicks one of the buttons in the Export Settings.

The ID of the button pressed is passed in ``exParamButtonRec.buttonParamIdentifier``.

Display any dialog using platform-specific UI, collect any user input, and save any changes back to ``privateData``.

If the user cancels the dialog, return ``expor­tReturn_ParamButtonCancel`` to signify that nothing in the ``privateData`` has changed.

----

exSelExport
================================================================================

- param1 - ``exDoExportRec*``
- param2 - ``unused``

Do the export! Sent when the user starts an export to the format supported by the exporter, or if the exporter is used in an Editing Mode and the user renders the work area.

Single file exporters are sent this selector only once per export (e.g. AVI, QuickTime). To create a single file, setup a loop where you request each frame in the startTime to endTime range using one of the render calls in the Sequence Render Suite and GetAudio in the Sequence Audio Suite. For better performance, you can use the asynchronous calls in the Sequence Render Suite to have the host render multiple frames on multiple threads.

Still frame exporters are sent ``exSelExport`` for each frame in the sequence (e.g. numbered TIFFs). The host will name the files appropriately.

Save render time by checking to see if frames are repeated. Inspect the SequenceRender_GetFrameReturnRec.repeatCount returned from a render call, which holds a frame repeat count.

----

exSelQueryExportFileExtension
================================================================================

- param1 - ``exQueryExportFileExtensionRec*``
- param2 - ``unused``

For exporters that support more than one file extension, specify an extension given the file type.

If this selector is not supported by the exporter, the extension is specified by the exporter in exEx­ porterInfoRec.fileTypeDefaultExtension.

----

exSelQueryOutputFileList
================================================================================

- param1 - ``exQueryOutputFileListRec*``
- param2 - ``unused``

For exporters that export to more than one file. This is called before an export for the host to find out which files would need to be overwritten.

It is called after an export so the host will know about all the files created, for any post encoding tasks, such as FTP. If this selector is not supported by the exporter, the host application will only know about the original path.

This selector will be called three times. On the first call, the plug-in fills out numOutputFiles. The host will then make numOutputFiles count of outputFileRecs, but empty.

On the second call, the plug-in fills out the path length (incl trailing null) for each exOutputFileRec element in outputFileRecs. The host will then allocate all paths in each outputFileRec.

On the third call, the plug-in fills in the path members of the outputFileRecs.

----

exSelQueryStillSequence
================================================================================

- param1 - ``exQueryStillSequenceRec*``
- param2 - ``unused``

The host application asks a still-only exporter if it wants to export as a sequence, and at what frame rate.

----

exSelQueryOutputSettings
================================================================================

- param1 - ``exQueryOutputSettingsRec*``
- param2 - ``unused``

The host application asks the exporter for general details about the current settings. This is a re- quired selector.

----

exSelValidateOutputSettings
================================================================================

- param1 - ``exValidateOutputSettingsRec*``
- param2 - ``unused``

The host application asks the exporter if it can export with the current settings.

The exporter should return ``exportReturn_ErrLastErrorSet`` if not, and the error string should be set to a description of the failure.

----

exSelEndInstance
================================================================================

- param1 - ``exExporterInstanceRec*``
- param2 - ``unused``

Deallocate any private data.

----

exSelShutdown
================================================================================

- param1 - ``unused``
- param2 - ``unused``

Sent immediately before shutdown. Free all remaining memory and close any open file handles.
