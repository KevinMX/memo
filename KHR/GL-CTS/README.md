# OpenGL / OpenGL ES CTS

以下步骤未验证

You're on your own.

```shell
sudo apt install -y mesa-common-dev cmake gcc g++ python3-lxml wget libpng-dev pkg-config
# https://github.com/KhronosGroup/VK-GL-CTS/releases/tag/opengl-es-cts-3.2.13.0
wget https://github.com/KhronosGroup/VK-GL-CTS/archive/refs/tags/opengl-es-cts-3.2.13.0.tar.gz
tar xvf opengl-es-cts-3.2.13.0.tar.gz
cd VK-GL-CTS-opengl-es-cts-3.2.13.0
python3 external/fetch_sources.py
mkdir build && cd build
#TODO: X11?
cmake .. -DDEQP_TARGET=null -DGLCTS_GTF_TARGET=gles32
cmake --build external/openglcts -j$(nproc)
cd ./external/openglcts/modules
#xy as major and minor specifiction versions
#see https://github.com/KhronosGroup/VK-GL-CTS/blob/main/external/openglcts/README.md#running-the-tests
./cts-runner --type=es32
```