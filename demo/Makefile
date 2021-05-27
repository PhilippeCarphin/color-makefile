include ../colors.mk

all: cfloat libcfloat.a libcfloat.so

# Executable targets
cfloat:cfloat.o
	$(call make_echo_link_c_executable)
	$(at) gcc $< -o $@
libcfloat.a:cfloat.o
	$(call make_echo_link_static_library)
	$(at) ar -crs $@ $^
libcfloat.so:cfloat.o
	$(call make_echo_link_shared_library)
	$(at) gcc -o $@ $^

# Testing targets
check: check_output libcfloat.a libcfloat.so
# Should not be used to compensate for deficient dependencies but has valid uses:
ccheck: clean check

check_output:cfloat_output pyfloat_output
	$(call make_echo_run_test,Comparing $^)
	$(at) diff $^
	$(call big_success)
cfloat_output: cfloat
	$(call make_echo_generate_file)
	$(at) ./$< | tee $@
pyfloat_output: pyfloat.py
	$(call make_echo_generate_file)
	$(at) python3 $< | tee $@
.PHONY:cfloat_output pyfloat_output

# Rules
%.o:%.c
	$(call make_echo_build_c_object)
	$(at) gcc -c -o$@ $<

clean:
	$(at) rm -f cfloat *_output *.o *.a *.so
