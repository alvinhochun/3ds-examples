all: libctru tracy


.PHONY: libctru
libctru: libctru/libctru/Makefile libctru/libctru/lib/libctru-prof.a


.PHONY: libctru/libctru/lib/libctru-prof.a
libctru/libctru/lib/libctru-prof.a:
	@$(MAKE) -C libctru/libctru lib/libctru-prof.a


.PHONY: tracy
tracy:
tracy: tracy-install/lib/libTracyClient.a


tracy-install/lib/libTracyClient.a: export CXXFLAGS=-I$(realpath libctru/libctru/include) -DGLIBCXX_GTHREAD_USE_WEAK=1
tracy-install/lib/libTracyClient.a: tracy/CMakeLists.txt libctru tracy-install/.dirstamp
	@[ -d tracy-install ] || mkdir -p tracy-install
	@catnip -S tracy -B tracy-build \
		-DCMAKE_INSTALL_PREFIX=$(realpath tracy-install) \
		-DTRACY_ENABLE=ON \
		-DTRACY_DELAYED_INIT=ON \
		-DTRACY_MANUAL_LIFETIME=ON \
		-DTRACY_NO_FRAME_IMAGE=ON \
		-DTRACY_SYMBOL_OFFLINE_RESOLVE=ON \
		-T3DS build
	@catnip -S tracy -B tracy-build \
		-T3DS install


tracy-install/.dirstamp:
	@[ -d tracy-install ] || mkdir -p tracy-install
	@touch $@


libctru/libctru/Makefile:
	@git submodule update --init libctru


tracy/CMakeLists.txt:
	@git submodule update --init tracy


clean:
	@[ ! -f libctru/libctru/Makefile ] || $(MAKE) -C libctru/libctru clean
	rm -rf tracy-build/
	rm -rf tracy-install/
