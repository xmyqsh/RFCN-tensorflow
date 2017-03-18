TF_INC := $(shell python -c 'import tensorflow as tf; print(tf.sysconfig.get_include())')

CUDA_PATH=/usr/local/cuda

all: roi_pooling.so

roi_pooling_grad.o: roi_pooling_grad.cc
	g++ -std=c++11  -D_GLIBCXX_USE_CXX11_ABI=1 -c -o roi_pooling_grad.o roi_pooling_grad.cc -I $(TF_INC) -fPIC -O2

roi_pooling.o: roi_pooling.cc
	g++ -std=c++11  -D_GLIBCXX_USE_CXX11_ABI=1 -c -o roi_pooling.o roi_pooling.cc -I $(TF_INC) -fPIC -O2

roi_pooling.cu.o: roi_pooling.cu.cc
	nvcc -std=c++11 -c -o roi_pooling.cu.o roi_pooling.cu.cc -I $(TF_INC) \
		-D GOOGLE_CUDA=1 -x cu -Xcompiler -fPIC -arch=sm_37

roi_pooling_grad.cu.o: roi_pooling_grad.cu.cc
	nvcc -std=c++11 -c -o roi_pooling_grad.cu.o roi_pooling_grad.cu.cc -I $(TF_INC) \
		-D GOOGLE_CUDA=1 -x cu -Xcompiler -fPIC -arch=sm_37


roi_pooling.so: roi_pooling.o roi_pooling_grad.o roi_pooling.cu.o roi_pooling_grad.cu.o
	g++ -std=c++11  -D_GLIBCXX_USE_CXX11_ABI=1 -shared -o roi_pooling.so \
		roi_pooling.o roi_pooling_grad.o roi_pooling.cu.o roi_pooling_grad.cu.o \
		-I $(TF_INC) -D GOOGLE_CUDA=1 -fPIC -O2 -lcudart -L $(CUDA_PATH)/lib64


clean:
	rm *.so
	rm *.o
