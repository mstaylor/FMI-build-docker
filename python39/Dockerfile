FROM amazonlinux:2

#ADD 3d5841a8ecf623746968e536377d8de2f31999d6efd32f39b6855aeaf4e8947b.tar.xz /
#ADD 41fc92f0393ae157b3c6ca6296383a765385e405988254fb3c4603e6836828e9.tar.xz /
#ADD 4894f6fd6e29b3dd865b0dbcc460d18fe4a49cfc1f4443951becd3dd7df1725d.tar.xz /
#ADD 5cb8cbe67380529dfaeb1c822904423e84e8ccca6fc64fea8e144749fda952a8.tar.xz /
#ADD 6390fc1113762e0e2909ea5333340b118cf952bf5f0df77a7294f1d2d9d6b704.tar.xz /
#ADD 9882e2c0b9128fab2c7fad5173cc2caf3e61ed106244b79bd055654d4f234afc.tar.xz /

ENV LANG=en_US.UTF-8
ENV TZ=:/etc/localtime
ENV PATH=/var/lang/bin:/usr/local/bin:/usr/bin/:/bin:/opt/bin
ENV LD_LIBRARY_PATH=/var/lang/lib:/lib64:/usr/lib64:/var/runtime:/var/runtime/lib:/var/task:/var/task/lib:/opt/lib
ENV LAMBDA_TASK_ROOT=/var/task
ENV LAMBDA_RUNTIME_DIR=/var/runtime

RUN yum -y groupinstall "Development Tools" && yum -y install openssl openssl-devel libcurl libcurl-devel wget gcc72 gcc72-c++ bzip2 which
RUN curl -O https://cmake.org/files/v3.18/cmake-3.18.0.tar.gz && tar -xvf cmake-3.18.0.tar.gz && cd cmake-3.18.0 && ./bootstrap && make -j6 install && cd .. && rm -rf cmake*
RUN wget https://boostorg.jfrog.io/artifactory/main/release/1.81.0/source/boost_1_81_0.tar.bz2
RUN tar --bzip2 -xf boost_1_81_0.tar.bz2 && \
    cd boost_1_81_0 && ./bootstrap.sh && ./b2 -d+2 cxxflags="-fPIC" link=static install
RUN git clone --recurse-submodules https://github.com/aws/aws-sdk-cpp.git && cd aws-sdk-cpp && mkdir build && cd build && \
    cmake -DCMAKE_BUILD_TYPE=Release -DBUILD_ONLY="s3" -DBUILD_SHARED_LIBS="off" .. && make -j6 install && cd ../.. && rm -rf aws-sdk-cpp
RUN git clone https://github.com/redis/hiredis.git && cd hiredis/ && make install && cd .. && rm -rf hiredis
