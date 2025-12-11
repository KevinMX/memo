# Vulkan CTS

以 TH1520 平台 RevyOS 为例。

截止编写时为 RevyOS 20251115，IMG DDK 25.1

---

### 编译 & 运行

```shell
#building natively on riscv64
sudo apt install -y git mesa-common-dev cmake gcc g++ python3-lxml pkg-config libpng-dev libvulkan-dev wget ninja-build
#https://github.com/KhronosGroup/VK-GL-CTS/releases/latest
wget https://github.com/KhronosGroup/VK-GL-CTS/archive/refs/tags/vulkan-cts-1.4.4.2.tar.gz
tar xvf vulkan-cts-1.4.4.2.tar.gz
cd VK-GL-CTS-vulkan-cts-1.4.4.2
python3 external/fetch_sources.py
mkdir build && cd build
#cmake .. -DCMAKE_BUILD_TYPE=Release
#cmake .. --DCMAKE_BUILD_TYPE=Debug
#make -j$(nproc)
cmake .. -GNinja -DCMAKE_BUILD_TYPE=Debug
ninja
#Build Mustpass (optional)
#python3 ../external/vulkancts/scripts/build_mustpass.py
#Run Vulkan CTS
cd external/vulkancts/modules/vulkan/
./deqp-vk --deqp-caselist-file=../../../../../external/vulkancts/mustpass/main/vk-default.txt \
--deqp-log-images=disable \
--deqp-log-shader-sources=disable
```

---

### 注意事项

#### 如果需要跑老版本 CTS
1.3.1.1 等老版本需要修改 external/fetch_sources.py，找到 zlib 的链接，替换成 [https://zlib.net/fossils/*](https://zlib.net/fossils/*)

若默认 GCC 无法编译通过可尝试 GCC 12/13（新版本一般不会有问题）

#### TH1520 平台已知无法通过的部分用例

已知部分用例会导致`deqp-vk`直接 Segfault 退出，可以用 waiver list 的方式跳过，在运行 deqp-vk 时加`--deqp-waiver-file=waiver.xml`参数

`vendorID`和`deviceID`可以通过`vulkaninfo 2>&1 | grep -E 'deviceID|vendorID'`获得

比如 TH1520 的 DDK 25.1 下，`deviceID = 0x36104182`，`vendorID = 0x1010`

注意：时间所限，下面这个列表还是很小一部分，可能还有非常多用例无法跑过，需要逐个测试

```xml
<?xml version="1.0" encoding="utf-8"?>
<waiver_list>

  <!--/*     Copyright (C) 2020 The Khronos Group Inc
  *
  *     Licensed under the Apache License, Version 2.0 (the "License");
  *     you may not use this file except in compliance with the License.
  *     You may obtain a copy of the License at
  *
  *          http://www.apache.org/licenses/LICENSE-2.0
  *
  *     Unless required by applicable law or agreed to in writing, software
  *     distributed under the License is distributed on an "AS IS" BASIS,
  *     WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
  *     See the License for the specific language governing permissions and
  *     limitations under the License.
  */-->
  <!--/*
  Each <waiver> entry must contain three attributes: vendorName, vendorId and url.
  Url should be a full path to gitlab issue(s).
  Waiver tag should have one <description> child that describes issue.
  Waiver tag should have one <device_list> child.
  Device list should have one or more <d> elements containing device ids for which this waiver was created.
  Waiver tag should contain one or more <t> elements containing test paths that should be waived.
  String in <t> can use wildcard *.

  <xs:schema xmlns:xs="http://www.w3.org/2001/XMLSchema">
  <xs:element name="waiver_list">
  <xs:complexType>
  <xs:sequence>
  <xs:element name="waiver" maxOccurs="unbounded">
  <xs:complexType>
  <xs:sequence>
  <xs:element name="description" type="xs:string"/>
  <xs:element name="device_list">
  <xs:complexType>
  <xs:sequence>
  <xs:element name="d" type="xs:integer" minOccurs="1" maxOccurs="unbounded"/>
</xs:sequence>
</xs:complexType>
</xs:element>
  <xs:element name="t" type="xs:string" minOccurs="1" maxOccurs="unbounded"/>
</xs:sequence>
  <xs:attribute name="vendorName" type="xs:string" use="required"/>
  <xs:attribute name="vendorId" type="xs:string" use="required"/>
  <xs:attribute name="url" type="xs:string" use="required"/>
</xs:complexType>
</xs:element>
</xs:sequence>
</xs:complexType>
</xs:element>
</xs:schema>
  */-->
  <waiver vendorName="Imagination Technologies" vendorId="0x1010" url="">
    <descrption>Segfault list.</description>
      <device_list>
        <d>0x36104182</d>
                </device_list>
                <t>dEQP-VK.api.object_management.private_data.device_memory_small</t>
                <t>dEQP-VK.api.object_management.private_data.buffer_uniform_small</t>
                <t>dEQP-VK.api.object_management.private_data.buffer_uniform_large</t>
                <t>dEQP-VK.api.object_management.private_data.buffer_storage_small</t>
                <t>dEQP-VK.api.object_management.private_data.buffer_storage_large</t>
                <t>dEQP-VK.api.object_management.private_data.buffer_view_uniform_r8g8b8a8_unorm</t>
                <t>dEQP-VK.api.object_management.private_data.buffer_view_storage_r8g8b8a8_unorm</t>
                <t>dEQP-VK.api.object_management.private_data.image_1d</t>
                <t>dEQP-VK.api.object_management.private_data.image_2d</t>
                <t>dEQP-VK.api.object_management.private_data.image_3d</t>
                <t>dEQP-VK.api.object_management.private_data.image_view_1d</t>
                <t>dEQP-VK.api.object_management.private_data.image_view_2d</t>
                <t>dEQP-VK.api.object_management.private_data.image_view_3d</t>
                <t>dEQP-VK.api.object_management.private_data.image_view_1d_arr</t>
                <t>dEQP-VK.api.object_management.private_data.image_view_2d_arr</t>
                <t>dEQP-VK.api.object_management.private_data.image_view_cube</t>
                <t>dEQP-VK.api.object_management.private_data.image_view_cube_arr</t>
                <t>dEQP-VK.api.object_management.private_data.pipeline_cache</t>
                <t>dEQP-VK.api.object_management.private_data.pipeline_layout_empty</t>
                <t>dEQP-VK.api.object_management.private_data.pipeline_layout_single</t>
                <t>dEQP-VK.api.object_management.private_data.query_pool</t>
                <t>dEQP-VK.api.object_management.private_data.render_pass</t>
                <t>dEQP-VK.api.object_management.private_data.sampler</t>
                <t>dEQP-VK.api.object_management.private_data.semaphore</t>
                <t>dEQP-VK.api.object_management.private_data.shader_module</t>
                <t>dEQP-VK.api.object_management.private_data.event</t>
                <t>dEQP-VK.api.object_management.private_data.fence</t>
                <t>dEQP-VK.api.object_management.private_data.fence_signaled</t>
                <t>dEQP-VK.api.object_management.private_data.framebuffer</t>
                <t>dEQP-VK.api.object_management.private_data.graphics_pipeline</t>
        </waiver>
</waiver_list>
```

