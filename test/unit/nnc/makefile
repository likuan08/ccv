include ../../../lib/config.mk

LDFLAGS := -L"../../../lib" -lccv $(LDFLAGS)
CFLAGS := -O3 -Wall -I"../../../lib" -I"../../" $(CFLAGS)
TARGETS = tfb.tests tensor.tests forward.tests backward.tests gradient.tests graph.tests winograd.tests transform.tests symbolic.graph.tests autograd.tests autograd.vector.tests while.tests

TARGET_SRCS := $(patsubst %,%.c,$(TARGETS))

.PRECIOUS: $(patsubst %,%.o,$(TARGETS))

.PHONY: release all test clean dep

release: all

include ../../../lib/scheme.mk

all: $(TARGETS)

test:
	@for test in $(TARGETS) ; do LLVM_PROFILE_FILE="$$test.profraw" ./"$$test" || exit ; done

clean:
	${MAKE} clean -C ../../../lib ; rm -f *.o $(TARGETS)

%.tests: %.tests.o libccv.a
	$(CC) -o $@ $< $(LDFLAGS)

libccv.a:
	${MAKE} -C ../../../lib

%.o: %.c
	$(CC) $< -o $@ -c $(CFLAGS)

.gitignore:
	echo $(TARGETS) | tr ' ' '\n' > .gitignore

dep: .dep.mk
.dep.mk: $(TARGET_SRCS)
	$(CC) $(CFLAGS) -MM $^ > .dep.mk

-include .dep.mk
