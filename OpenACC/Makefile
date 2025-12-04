# Choose an OpenACC-capable Fortran compiler.
# On many clusters this is nvfortran (from NVIDIA HPC SDK).
FC = nvfortran

# Compilation flags:
#  -acc          : enable OpenACC
#  -Minfo=accel  : show info about OpenACC optimizations
#  -fast         : general optimizations
#  -I. ...       : same netCDF include flags as before
FCFLAGS = -acc -Minfo=accel -fast -I. `nf-config --fflags`

# Link flags:
#  -acc          : enable OpenACC at link time as well
#  -L... ...     : same netCDF libs as before
LDFLAGS = -acc -L`nc-config --libdir` `nf-config --flibs`

all: model

model : model.F90 module_parameters.o module_types.o \
        module_physics.o module_output.o
	$(FC) $(FCFLAGS) $< -o $@ *.o $(LDFLAGS)

module_parameters.o : module_parameters.f90
	$(FC) $(FCFLAGS) $< -c

module_types.o : module_types.F90 module_parameters.o
	$(FC) $(FCFLAGS) $< -c

module_output.o : module_output.F90 module_physics.o
	$(FC) $(FCFLAGS) $< -c

module_physics.o : module_physics.f90 module_types.o
	$(FC) $(FCFLAGS) $< -c

clean :
	rm -f *.o *.mod model *.nc
