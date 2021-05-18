# Installing .NET 5.0 on Raspberry Pi 4

None of the official methods worked for me \(not even [involving horrid fixes](https://github.com/dotnet/core/issues/4446#issuecomment-843162084) ;\)\). So I ended up going the super manual route \(mostly copying [https://elbruno.com/2019/08/27/raspberrypi-how-to-install-dotnetcore-in-a-raspberrypi4-and-test-with-helloworld-of-course/](https://elbruno.com/2019/08/27/raspberrypi-how-to-install-dotnetcore-in-a-raspberrypi4-and-test-with-helloworld-of-course/) and [https://elbruno.com/2020/01/05/raspberrypi-how-to-solve-dotnet-core-not-recognized-after-reboot/](https://elbruno.com/2020/01/05/raspberrypi-how-to-solve-dotnet-core-not-recognized-after-reboot/)\).

```text
$ sudo apt-get install lshw
$ sudo lshw
    description: Computer
    product: Raspberry Pi 4 Model B Rev 1.4
    serial: 10000000d5e618a2
    width: 64 bits

; OK, it's 64bits
; Get Arm64 download URL from https://dotnet.microsoft.com/download/dotnet/5.0

$ mkdir temp && cd temp
$ curl [URL] --output [FILENAME]
$ mkdir $HOME/dotnet
$ sudo tar zxf [FILENAME] -C $HOME/dotnet/
$ sudo ln -s $HOME/dotnet/dotnet /usr/local/bin
$ dotnet --version
5.0.203 
; or whatever version you downloaded

; Now we need to add it to PATH. Add the exports
$ sudo nano ~/.bashrc

export DOTNET_ROOT=$HOME/dotnet
export PATH=$PATH:$HOME/dotnet
export PATH=$PATH:$HOME/.dotnet/tools

; then Ctrl+X to save and exit. finally, load the new exports
$ source ~/.bashrc
```





