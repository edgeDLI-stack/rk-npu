# npu demo

-   ref
    -   https://wiki.t-firefly.com/zh_CN/ROC-RK3588S-PC/usage_npu.html
    -   https://blog.csdn.net/weixin_42829652/article/details/122250357

> 非 rknn 模型转换

-   注意 RKNN-Toolkit2 目前版本适用系统`Ubuntu18.04(x64)`及以上
    -   开发板上无法直接安装
-   安装在 ubuntu-1 上
-   RKNN-Toolkit2 自带了一个模拟器，直接在 ubuntu-1 上运行 Demo 即是将转换后的模型部署到仿真 NPU 上运行
    -   需要安装 cv2、protobuf 依赖
        -   apt-get install ffmpeg libsm6 libxext6 -y
        -   apt-get install libprotobuf-dev protobuf-compiler -y
-   也可以转换后，运行在 3588 npu 上
    -   注意导出 rknn 模型需要指定 config 参数为 target_platform="rk3588"（默认是导出 356X，否则存在兼容性问题）

```shell
adb push rknpu2/runtime/RK3588/Linux/rknn_server/aarch64/usr/bin/rknn_server /usr/bin/
adb push rknpu2/runtime/RK3588/Linux/librknn_api/aarch64/librknnrt.so /usr/lib/
adb push rknpu2/runtime/RK3588/Linux/librknn_api/aarch64/librknn_api.so /usr/lib/

# 可以使用 "systemctl status rknn_server" 查看rknn_server服务是否处于运行状态
# 若没有运行，请在板子的串口终端运行rknn_server
chmod +x /usr/bin/rknn_server
/usr/bin/rknn_server
```

> rknn 模型

```shell
cd ~/py_project/RK_NPU_SDK_1.2.0/release/rknpu2/examples/rknn_mobilenet_demo

# 编译生成linux demo
bash build-linux_RK3588.sh

cd install/rknn_mobilenet_demo_Linux

# export生成的lib库
export LD_LIBRARY_PATH=./lib
# 运行
./rknn_mobilenet_demo model/RK3588/mobilenet_v1.rknn model/dog_224x224.jpg
```
