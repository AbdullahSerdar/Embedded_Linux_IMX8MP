# Embedded_Linux_IMX8MP_QGC

[EN] I aimed to lay the foundations of a ground control station using Embedded Linux. In this process, I wanted to demonstrate how structures such as adding layers and cross-compiling are built. I did not share the full coding part of the project, since every system has different requirements, which significantly reduces the likelihood of the code working as-is. The purpose of this page is to illustrate, through my project, the process that someone working with Embedded Linux would go through and the requirements they would need.

[TR] Gömülü Linux kullanarak bir yer kontrol istasyonunun temellerini atmayı hedefledim. Bu süreçte, katman ekleme ve çapraz derleme gibi yapıların nasıl oluşturulduğunu göstermek istedim. Her sistemin farklı gereksinimleri olduğundan ve kodun olduğu gibi çalışma olasılığı önemli ölçüde azaldığından, projenin tüm kodlama kısmını paylaşmadım. Bu sayfanın amacı, projem aracılığıyla, Gömülü Linux ile çalışan birinin izleyeceği süreci ve ihtiyaç duyacağı gereksinimleri göstermektir.

I recommend you watch the tutorial given by Murat Boyar for Yocto.(https://www.youtube.com/playlist?list=PLlKf4EyiSijBz9dsXVGe7a-R6wJCvSRXt)

<img width="447" height="381" alt="Picture1" src="https://github.com/user-attachments/assets/25b9b808-320c-493e-a890-13356de1389b" />

Torizon's website that explains how to compile our own image(https://developer.toradex.com/linux-bsp/os-development/build-yocto/build-a-reference-image-with-yocto-projectopenembedded/)

The source code of the QGC (https://github.com/mavlink/qgroundcontrol/tree/Stable_V4.4) 

If you have any questions, you can look for answers through the Toradex Community.(https://community.toradex.com/)

NOTE: If a white screen appears after the image is loaded, or if data is sent to the screen but a clear background does not form, the reason may be artifacts generated during compilation. To fix this, access the board’s serial terminal and enter: "systemctl disable wayland-app-launch.service", then reset the board. If the issue persists, repeat the process.
