.. _gpu-effects-transitions/gpu-effects-transitions:

GPU Effects & Transitions
################################################################################

This chapter describes the additional capabilities available to effects and transitions for GPU interoperability with Premiere Pro. The GPU extensions allow these plug-ins to have full access to GPU-resident frames without readback to system memory, when using the Mercury Playback Engine in a GPU-accelerated mode. Effects and transitions can also optionally tell the host that they support real-time processing, so that they will not be flagged as non-realtime.

The GPU extensions work on top of effects and transitions built using the After Effects SDK. The extensions are designed to supplement a regular software effect or transition, which defines the software rendering path, parameters, custom UI drawing, and other standard interaction. The GPU effect exists as a new entry point for rendering using the GPU if possible. The software render path will be used otherwise.

----

System Requirements
================================================================================

The system requirements for developing GPU effects & transitions are higher than developing other plug-ins. You'll need a video card that supports Mercury Playback Engine GPU-

acceleration. Make sure your video card supports the type of video acceleration you are developing, on the platform you are developing on. See this page for the latest supported video cards: https://helpx.adobe.com/premiere-pro/system-requirements.html

The CUDA SDK is also needed for CUDA rendering development.

