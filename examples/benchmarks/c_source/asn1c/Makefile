ASN1C ?= ../asn1c-0.9.28/asn1c/asn1c
ASN1C_FLAGS = -fcompound-names -fincludes-quoted -gen-PER

all:
	gcc $(CFLAGS) $(OPT_SIZE) generated/*.c main.c -o main
	size main
	readelf -s -W main | sort -k3 -n
	valgrind $(MASSIF_FLAGS) --massif-out-file=main.massif ./main 1
	ms_print main.massif
	time ./main $(ENCODE_DECODE_ITERATIONS)
	gcc $(CFLAGS) $(OPT_SPEED) generated/*.c main.c -o main
	time ./main $(ENCODE_DECODE_ITERATIONS)

generate: asn1c-0.9.28
	rm -rf generated
	mkdir -p generated
	cd generated && \
	    $(ASN1C) $(ASN1C_FLAGS) ../../my_protocol.asn && \
	    rm Makefile.am.sample converter-sample.c

v0.9.28.tar.gz:
	wget https://github.com/vlm/asn1c/archive/v0.9.28.tar.gz

asn1c-0.9.28: v0.9.28.tar.gz
	rm -rf asn1c-0.9.28
	tar xf v0.9.28.tar.gz
	cd asn1c-0.9.28 && \
	    test -f configure || autoreconf -iv && \
	    ./configure && \
	    $(MAKE)

include ../common.mk
