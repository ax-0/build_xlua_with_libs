复制xlua2.15的build文件夹源码到build文件下 CMakeLists.txt除外

xlua.c文件增加如下代码：

#define LUA_RIDX_UPDATE				22
#define LUA_RIDX_LATEUPDATE			23
#define LUA_RIDX_FIXEDUPDATE		24
#define LUA_RIDX_CUSTOMTRACEBACK 	28
#define lua_getref(L,ref)       lua_rawgeti(L, LUA_REGISTRYINDEX, (ref))

LUALIB_API int xlua_update(lua_State *L, float deltaTime, float unscaledTime)
{
	xlua_getglobal(L, "Update");
	lua_pushnumber(L, deltaTime);
	lua_pushnumber(L, unscaledTime);
	return lua_pcall(L, 2, -1, 0);
}

LUALIB_API int xlua_lateupdate(lua_State *L)
{
	xlua_getglobal(L, "LateUpdate");
	return lua_pcall(L, 0, -1, 0);
}

LUALIB_API int xlua_fixedupdate(lua_State *L, float fixedTime)
{
	xlua_getglobal(L, "FixedUpdate");
	lua_pushnumber(L, fixedTime);
	return lua_pcall(L, 0, -1, 0);
}

删除build下所有以build开头的子文件夹 防止编译失败或错误

Win10
安装官网CMake
安装Visual Studio 2017 包含C++和移动C++(Android和iOS)
到此步骤一般可以编译Win平台的Dll了

安装Android Studio 从Configure->SDKManager->SDKTools安装CMake工具 勾选右下角Show Package Details 安装旧版CMake(3.6.4xxx)
安装AndroidSDK
新建环境变量ANDROID_SDK 值为AndroidSDK路径
从官网下载android_ndk_r13b然后解压放到任意目录
新建环境变量ANDROID_NDK 值为ndroid_ndk_r13b路径
到此步骤一般可以编译Android平台so了

Mac
安装官网CMake 再使用terminal安装CMake的命令行工具(sudo "/Applicaiton/CMake.app/COmtents/bin/cmake-gui" --install)
检查XCode是否为beta版本 将名称不是XCode的都改名为XCode 如果无法识别XCode时运行命令(sudo /usr/bin/xcode-select --switch /Applications/Xcode.app) 路径根据自己安装的不同而不同
到此步骤mac上就能编译mac或者iOS的动态库了
