# templight for clang 3.9 release  

For more details you can visit *https://github.com/mikael-s-persson/templight* the original source  
I'll just detail how to build correctly since the original is outdated.

# Build routine on UNIX-LIKE systems  

Let ${LLVM_SOURCE} be our build folder  

# - get 3.9 llvm from source  
svn co http://llvm.org/svn/llvm-project/llvm/tags/RELEASE_390/final llvm  
result should be -> Checked out revision 286942.  
  
# - get 3.9 clang from source  
cd ${LLVM_SOURCE}/llvm/tools  
svn co http://llvm.org/svn/llvm-project/cfe/tags/RELEASE_390/final clang  
result should be -> Checked out revision 286943.  
  
# - get 3.9 compiler-rt from source  
cd ${LLVM_SOURCE}/llvm/projects  
svn co http://llvm.org/svn/llvm-project/compiler-rt/tags/RELEASE_390/final compiler-rt  
result should be -> Checked out revision 286944.  
  
# - get templight   
cd ${LLVM_SOURCE}/llvm/tools/clang/tools  
git clone https://github.com/DkatoNaB/templight.git templight  

# - build with cmake
*NOTE: personally I built it with clang 3.8 but you can use gcc instead if you insist*  
  
cd ${LLVM_SOURCE}/llvm/tools/clang  
svn patch tools/templight/templight_clang_patch.diff  
*NOTE: if you have some .rej files you can manually apply the diffs. It shouldn't be more than 5 rows.*  

cd ${LLVM_SOURCE}/llvm/tools/clang/tools
add the *add_clang_subdirectory(templight)* to the CMakeLists.txt

cd ${LLVM_SOURCE}  
mkdir build && cd build  
CXX=clang++ CC=clang CXXFLAGS=-std=c++11 cmake ../llvm -DCMAKE_BUILD_TYPE=RelWithDebInfo  
make clang  
  
if done without erros you are free to  
cd build/llvm/tools/clang/tools/templight  
make  
  
At this point you should have clang and templight executables

