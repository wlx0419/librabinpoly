AUTOMAKE_OPTIONS = foreign
# ACLOCAL_AMFLAGS = -I m4

# SUBDIRS = doc man script src
SUBDIRS = src test python

#
# tests
# 

BENCHFN=/tmp/rabin-benchmark.test
export LD_LIBRARY_PATH := $(CURDIR)/src/.libs:$(LD_LIBRARY_PATH)
export PYTHONPATH := $(CURDIR)/python:$(PYTHONPATH)

.PHONY: test 

test: test/random-42x1M.dat
	test/test_load.py
	test/test_16_32_64.py
	test/test_reset.py
	test/test_zeros.py
	test/test_ones.py
	test/test_next.py
	test/test_hash.py
	test/test_eof.py


$(BENCHFN): 
	dd if=/dev/urandom of=$(BENCHFN) bs=1M count=10

benchmark: $(SO) $(BENCHFN)
	test/benchmark.py $(BENCHFN)

test/random-42x1M.dat:
	test/mkrandom 42 1048576 > $@

lint:
	cppcheck --enable=all src/*.c src/*.h
	make clean && scan-build -V make

