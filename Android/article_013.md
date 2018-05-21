# selectedIndex 失效问题
1：列出模拟器类型
`android list targets`

2：建立模拟器
`android create avd --target 2 --name cupcake (cupcake)`为新建模拟器的名字

3：列出自己建立的么模拟器
`android list avd`

4：切换模拟器样式
在创建命令后面加上 “--skin QVGA”即可
切换样式：Windows操作系统按“F7”键即可

5：删除模拟器
`android delete avd --name cupcake (cupcake)`为删除的模拟器的名字

6：指定用什么模拟器启动
`emulator -debug avd_config -avd cupcake (cupcake)`为模拟器的名字