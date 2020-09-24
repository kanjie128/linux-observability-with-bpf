CLANG = clang

EXECABLE = monitor-exec

BPFCODE = bpf_program

BPFTOOLS = /kernel-src/samples/bpf
BPFLOADER = $(BPFTOOLS)/bpf_load.c
BPFTEST = /kernel-src/tools/testing/selftests/bpf
BPFHELPER = $(BPFTEST)/trace_helpers.c

CCINCLUDE += -I/kernel-src/tools/testing/selftests/bpf

LOADINCLUDE += -I/kernel-src/samples/bpf
LOADINCLUDE += -I/kernel-src/tools/lib
LOADINCLUDE += -I/kernel-src/tools/perf
LOADINCLUDE += -I/kernel-src/tools/include
LIBRARY_PATH = -L/usr/local/lib64
NOATTRTEST = -DHAVE_ATTR_TEST=0
BPFSO = -lbpf

.PHONY: clean bpfload build

clean:
	rm -f *.o *.so $(EXECABLE)

build: ${BPFCODE.c} ${BPFLOADER}
	$(CLANG) -O2 -target bpf -c $(BPFCODE:=.c) $(CCINCLUDE) -o ${BPFCODE:=.o} $(NOATTRTEST)

bpfload: build
	clang -o $(EXECABLE) -lelf $(LOADINCLUDE) $(LIBRARY_PATH) $(BPFSO) $(CCINCLUDE) $(NOATTRTEST) \
        $(BPFLOADER) $(BPFHELPER) loader.c

$(EXECABLE): bpfload

.DEFAULT_GOAL := $(EXECABLE)
