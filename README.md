# Embedded_Linux_IMX8MP

[EN]I aimed to lay the foundation of a ground control station using Embedded Linux. During this process, I wanted to demonstrate how structures such as adding layers and 
cross-compiling are built. The shared code in the project will work only to a limited extent if used as-is. The purpose of this page is to provide a foundation for the Embedded Linux system.

[TR]Embedded Linux kullanarak bir yer kontrol istasyonunun temellerini atmayı hedefledim. Bu süreçte, katman ekleme ve çapraz derleme gibi yapıların nasıl oluşturulduğunu
göstermek istedim. Projede paylaşılan kodları hazır olarak kullandığınızda çalışması oldukça azdır. Bu sayfanın amacı Gömülü Linux sistemi için bir temel oluşturmaktır.

I recommend you watch the tutorial given by Murat Boyar for Yocto.(https://www.youtube.com/playlist?list=PLlKf4EyiSijBz9dsXVGe7a-R6wJCvSRXt)

Torizon's website that explains how to compile our own image(https://developer.toradex.com/linux-bsp/os-development/build-yocto/build-a-reference-image-with-yocto-projectopenembedded/)

The source code of the QGC (https://github.com/mavlink/qgroundcontrol/tree/Stable_V4.4) 

NOTE: If a white screen appears after the image is loaded, or if data is sent to the screen but a clear background does not form, the reason may be artifacts generated during compilation. To fix this, access the board’s serial terminal and enter: "systemctl disable wayland-app-launch.service", then reset the board. If the issue persists, repeat the process.
