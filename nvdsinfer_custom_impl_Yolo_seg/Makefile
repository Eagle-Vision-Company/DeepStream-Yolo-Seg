CUDA_VER?=
ifeq ($(CUDA_VER),)
	$(error "CUDA_VER is not set")
endif

CXX:= g++
NVCC:=/usr/local/cuda-$(CUDA_VER)/bin/nvcc

TARGET_LIB:= libnvdsinfer_custom_impl_Yolo_seg.so

CFLAGS:= -Wall -std=c++17 -shared -fPIC -Wno-error=deprecated-declarations

CFLAGS+= -I/opt/nvidia/deepstream/deepstream/sources/includes -I/usr/local/cuda-$(CUDA_VER)/include
CUFLAGS:= -I/opt/nvidia/deepstream/deepstream/sources/includes -I/usr/local/cuda-$(CUDA_VER)/include

LIBS+= -L/usr/local/cuda-$(CUDA_VER)/lib64

LFLAGS:= -shared -Wl,--start-group $(LIBS) -Wl,--end-group

SRCS:= $(wildcard *.cpp)
SRCS+= $(wildcard trt_plugins/*.cpp)
SRCS+= $(wildcard trt_plugins/efficientNMSPlugin/*.cpp)
SRCS+= $(wildcard trt_plugins/efficientNMSPlugin/*.cu)
SRCS+= $(wildcard trt_plugins/roiAlignPlugin/*.cpp)
SRCS+= $(wildcard trt_plugins/roiAlignPlugin/*.cu)

INCS:= $(wildcard *.h)
INCS+= $(wildcard trt_plugins/*.h)
INCS+= $(wildcard trt_plugins/efficientNMSPlugin/*.h)
INCS+= $(wildcard trt_plugins/efficientNMSPlugin/*.cuh)
INCS+= $(wildcard trt_plugins/roiAlignPlugin/*.h)
INCS+= $(wildcard trt_plugins/roiAlignPlugin/*.cuh)

OBJS:= $(addsuffix .o, $(basename $(SRCS)))

all: $(TARGET_LIB)

%.o: %.cpp Makefile
	$(CXX) -c -o $@ $(CFLAGS) $<

%.o: %.cu $(INCS) Makefile
	$(NVCC) -c -o $@ --compiler-options '-fPIC' $(CUFLAGS) $<

$(TARGET_LIB) : $(OBJS)
	$(CXX) -o $@ $(OBJS) $(LFLAGS)

clean:
	rm -rf $(OBJS) $(TARGET_LIB)
