FROM debian:sid
MAINTAINER Antoine Gaillard <antoine.gaillard13@gmail.com>

RUN apt-get update && apt-get install -y libstdc++-5-dev --no-install-recommends \
	&& rm -rf /var/lib/apt/lists/*
RUN buildDeps=' \
		g++ \
		cmake \
		git \
		libffi-dev \
		make \
		pkg-config \
		python \
	' \
	&& apt-get update && apt-get install -y $buildDeps --no-install-recommends \
	&& rm -rf /var/lib/apt/lists/* \
	&& mkdir /src \
	&& cd /src \
	&& git clone --depth 1 http://root.cern.ch/git/llvm.git --branch cling-patches \
	&& git clone --depth 1 http://root.cern.ch/git/clang.git --branch cling-patches \
	&& git clone --depth 1 http://root.cern.ch/git/cling.git --branch master \
	&& ln -s /src/clang llvm/tools \
	&& ln -s /src/cling llvm/tools \
	&& mkdir build \
	&& cd build \
	&& cmake \
		-DCMAKE_BUILD_TYPE=Release \
		-DCMAKE_INSTALL_PREFIX="/usr/local" \
		-DLLVM_BUILD_LLVM_DYLIB=OFF \
		-DLLVM_ENABLE_CXX1Y=ON \
		-DLLVM_ENABLE_FFI=ON \
		-DLLVM_ENABLE_RTTI=ON \
		-DLLVM_TARGETS_TO_BUILD="X86" \
		../llvm \
	&& make \
	&& make -C tools/clang install \
	&& make -C tools/cling install \
	&& rm -rf /src \
	&& apt-get purge -y $buildDeps --auto-remove \
	&& update-alternatives --install /usr/bin/c++ c++ /usr/local/bin/clang 1

ENTRYPOINT [ "cling" ]
