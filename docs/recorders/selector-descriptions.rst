.. _recorders/selector-descriptions:

Selector Descriptions
################################################################################

This section provides a brief overview of each selector and highlights implementation issues.

----

recmod_Startup8
================================================================================

- param1 - ``recInfoRec8``
- param2 - ``unused``

Sent once when Premiere launches so the plug-in can initialize and return its attributes to Premiere.

The module should connect to any required capture hardware or drivers and fill in the ``recInfoRec8``.

----

recmod_Shutdown
================================================================================

- param1 - ``unused``
- param2 - ``unused``

Sent when Premiere terminates. Deallocate any memory and release the capture hardware or driver.

----

recmod_GetSetupInfo8
================================================================================

- param1 - ``PrivateData``
- param2 - ``recCapSetups8``

Sent when the Capture Settings dialog is first displayed to obtain custom settings information.

recCapSetups8 provides text label fields button titles and enabling.

----

recmod_ShowOptions
================================================================================

- param1 - ``PrivateData``
- param2 - ``recSetupParms``

Sent when the user presses a settings button (one of four available) in the Capture Settings dialog.

Request settings buttons during ``recmod_GetSetupInfo8``.

``recSetupParms`` indicates which button was pushed.

If the char* passed in ``recSetupParms`` isn't NULL, it points to memory containing private data; otherwise, no previous settings are available.

All the setup dialogs share the same memory; only one record is preserved.

If there are several different setup records, they should all fit within one flattened memory allocation.

----

recmod_Open
================================================================================

- param1 - ``PrivateData``
- param2 - ``recOpenParms``

Sent when Premiere's New Project Settings > Capture Settings dialog or the Movie Capture window is displayed.

Initialize hardware, create a private data structure for instance data, and pass a pointer to it back in ``param1``.

It will be sent back to you with subsequent selectors. ``recOpenParms`` contains information about the capture window and callbackID; store this information in private instance data.

----

recmod_Close
================================================================================

- param1 - ``PrivateData``
- param2 - ``unused``

Capture is complete and the capture window is closed. Disconnect from the hardware and deallocate the private instance data.

----

recmod_SetActive
================================================================================

- param1 - ``PrivateData``
- param2 - ``boolean toggle``

param2 indicates whether the plug-in should activate. When a capture window is opened or receives the focus, it will be activated.

----

recmod_SetDisp
================================================================================

- param1 - ``PrivateData``
- param2 - ``recDisplayPos``

Sent when the capture window is resized or moved. Update a proxy or overlay in the capture window during capture.

``recDisplayPos`` specifies the new bounds.

If they are unacceptable, modify them; the selector will be sent again with the new position.

Set mustresize in ``recDisplayPos`` to resize the preview frame with the specified bounds.

The plug-in is not allowed to resize the capture window, just the preview frame.

If mustresize is set but the plug-in can't resize the frame, display something (black, grey, a graphic of your choice) for a preview.

``mustresize`` will be set when the Capture Settings dialog is being displayed.

----

recmod_Idle
================================================================================

- param1 - ``PrivateData``
- param2 - ``recGetTimecodeRec*``

Sent to give the plug-in processing time.

----

recmod_PrepRecord8
================================================================================

- param1 - ``PrivateData``
- param2 - ``recCapParmsRec8``

Set up for recording, based on the data in recInfoRec8. If the prerollFunc callback function pointer is valid, call it to tell the device controller to get the device ready.

Recording commences with the next selector, ``recmod_StartRecord``.

If pressing the record button results in a recorder error before the ``recmod_PrepRecord8`` selector is even sent, make sure that the fileType four character code set in ``recInfoRec8`` is supported by an installed importer.

----

recmod_StartRecord
================================================================================

- param1 - ``PrivateData``
- param2 - ``recCapturedFileInfo*``

Sent after ``recmod_PrepRecord``. Start capturing immediately.

The pointer to ``recCapturedFileInfo`` is valid until the recording finishes.

----

recmod_ServiceRecord
================================================================================

- param1 - ``PrivateData``
- param2 - ``unused``

Sent repeatedly to give the plug-in processor time while recording.

----

recmod_StopRecord
================================================================================

- param1 - ``PrivateData``
- param2 - ``unused``

Stop recording and release record buffers.

----

recmod_CloseRecord
================================================================================

- param1 - ``PrivateData``
- param2 - ``unused``

Sent after ``recmod_StopRecord``.

During batch capturing, ``recmod_StopRecord`` will be called after every clip, but ``recmod_CloseRecord`` will not be called until after the last clip has been captured, to finalize the record process.

----

recmod_QueryInfo
================================================================================

- param1 - ``PrivateData``
- param2 - ``recCapInfoRec*``

Sent when the user hits the Log Clip button in the Capture panel.

The recorder should provide the dimensions, pixel aspect ratio, and other attributes to be assigned to the offline clip.

If the dimensions are not provided, the maxWidth/maxHeight values set in ``recInfoRec8`` will be used, which may be incorrect if the recorder supports multiple video resolutions.
