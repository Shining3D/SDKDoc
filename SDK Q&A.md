# SDK Q&A

## Q: How to launch to Sn3DPlatform?

A:

Sn3DPlatform is an executable which can be launched directly by double-click the exe file. Developers can also use language-specific commands to start the exe indirectly.

## Q: How to communicate with Sn3DPlatform?

A:

You should setup at least 2 ZMQ sockets to communicate with Sn3DPlatform:

1. SUB socket. It should connect to the port of Sn3DPlatform's PUB socket to receive any broadcast information published by the SDK platform.
2. REQ socket. It should connect to the port of Sn3DPlatform's REP socket to send any request to control the device.

**ATTENTION**: You **MUST** call `v1.0/device/check` via the REQ socket before any other requests, and check the results of the request to make sure the USB and GPU are both OK and the device is online.

If you want to resolve the scanned data like point clouds, wrapped meshes etc, you should setup a 3rd ZMQ socket to wait for request from the SDK platform. Unlike the first two sockets, the 3rd socket is a REP socket, which means you should get an available port and bind the port to the socket to wait for the connecting and requests from the SDK. You should send the URL of this socket to the SDK platform under the interface `v1.0/scan/register` via the REQ socket to the platform. And after that, when any big data is ready, the platform will send request to your REP socket with necessary information of the big data. Your app will be able to open the shared memory and analyze the data, then reply back tot he platform to finish the request-reply pattern.

Suppose that port 11200 is available on the computer, then the workflow is shown below:

![Big data analyze flow](assets/big_data_analyze.jpg)

## Q: Why does the creating or opening projects fail?

A:

Please make sure that you have set one of the three scanning modes (fixed, HD, rapid) before creating or opening any projects. You can use the Request Set interface `v1.0/scan/type/set` and provide the coresponding scan type to make the device enter certain scanning type mode.

## Q: How to change different scanning types?

A:

You should first exit scanning before changing other scanning types. Then you can use the Request Set interface `v1.0/scan/type/set` and provide the coresponding scan type to make the device enter certain scanning type mode.