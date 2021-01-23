ORB_SLAM3 IN WINDOWS 10, VISUAL STUDIO 2019 

# 1. References
- ORB-SLAM3 original project: https://github.com/UZ-SLAMLab/ORB_SLAM3
- ORB-SLAM3 for Windows: https://github.com/chanho-code/ORB-SLAM3forWindows
- ORB-SLAM2 for Windows: https://github.com/Phylliida/orbslam-windows

# 2. Prerequisites
- Visual Studio: tested with Visual Studio 2019
- CMake GUI: tested with 3.18.2
- boost: tested with 1.67.0 https://www.boost.org/users/history/version_1_67_0.html
- OpenCV: required at leat 3.0, tested with 3.4.13 https://opencv.org/releases/

# 3. Build thirdparty libraries

## DBoW2
- Included in Thirdparty
- Create a 'build' directory in Thirdparty/DBoW2  
- Open CMake GUI, click Configure and select Visual Studio 16 2019 
- Enter value OpenCV_DIR: path to OpenCV build
- Click Configure until no variables are considered new, Generate, open project in Visual Studio
- Change build type to Release 
- Open project properties 
	-> General tab -> Configure Type: Static Library (.lib)
	-> Advanced tab -> Target File Extension: .lib
	-> C/C++ Tab -> Code Generation -> Runtime Library: Multi-threaded(/MT)
- Build ALL_BUILD. There are some warnings but no errors.
- Build succeeded Thirdparty/DBoW2/lib/Release/DBoW2.lib

## eigen
- Included in Thirdparty
- Required by g2o

## g2o
- Included in Thirdparty
- Create a 'build' directory in Thirdparty/g2o
- Open CMake GUI, click Configure and select Visual Studio 16 2019  
- Click Generate and open project in Visual Studio
- Change build type to Release
- Open project properties 
	-> General tab -> Configure Type: Static Library (.lib)
	-> Advanced tab -> Target File Extension: .lib
	-> C/C++ Tab -> Code Generation -> Runtime Library: Multi-threaded(/MT)
	-> C/C++ Tab -> Preprocessor -> Preprocessor Definitions: add a 'WINDOWS' on top 
- Build ALL_BUILD. There are some warnings but no errors.
- Build succeeded Thirdparty/g2o/build/Release/g2o.lib

## Pangolin
- Included in Thirdparty
- Create a 'build' directory in Thirdparty/Pangolin
- Open CMake GUI, click Configure and select Visual Studio 16 2019
- Click Congifure until no variables are considered new, Generate and open project in Visual Studio
- Change build type to Release
- Open project properties 
	-> C/C++ Tab -> Code Generation -> Runtime Library: Multi-threaded(/MT) 
- Build ALL_BUILD. Ignore the error 'cannot open input file pthread.lib'
- Build succeeded ThirdParty/Pangolin/lib/Release/pangolin.lib

# 4. BUILD ORB-SLAM3
- Create a 'build' directory in ORB-SLAM3
- Open CMake GUI, click Configure and select Visual Studio 16 2019
- Enter value OpenCV_DIR: path to OpenCV build
- Click Congifure until no variables are considered new, Generate and open project in Visual Studio
- In system.h and system.cc of ORB_SLAM3 project: define 'usleep' function (from https://stackoverflow.com/questions/5801813/c-usleep-is-obsolete-workarounds-for-windows-mingw) which is used in examples
- Change build type to Release
- Set boost include and lib (can be done in CMakeLists)
- Open project properties 
	-> General tab -> Configure Type: Static Library (.lib)
	-> Advanced tab -> Target File Extension: .lib
	-> C/C++ Tab -> Preprocessor -> Preprocessor Definitions: add COMPILEDWITHC11
	-> C/C++ Tab -> Code Generation -> Runtime Library: Multi-threaded(/MT)
- Build ORB-SLAM3
- Build succeeded ORB_SLAM3/build/Release/ORB-SLAM3.lib

# 5. BUILD examples
## stereo_inertial_tum_vi
- Change build type to Release
- Open project properties
	-> C/C++ Tab -> Preprocessor -> Preprocessor Definitions: add COMPILEDWITHC11
	-> C/C++ tab -> Code Generation -> Runtime Library: Multi-threaded(/MT)
	-> Linker tab -> Advanced -> delete path in Import library
- Build stereo_inertial_tum_vi
- Build succeeded ORB_SLAM3/Examples/Stereo-Inertial/Release/stereo_inertial_tum_vi.exe
- Extract ORBvoc.txt.tar.gz to ORBvoc.txt
- Run: Examples\Stereo-Inertial\Release\stereo_inertial_tum_vi
	path_to_vocabulary
	path_to_settings
	path_to_image_folder_1
	path_to_image_folder_2
	path_to_times_file
	path_to_imu_data
	trajectory_file_name