TOP=../..

include $(TOP)/configure/CONFIG
#----------------------------------------
#  ADD MACRO DEFINITIONS BELOW HERE

# xxxRecord.h will be created from xxxRecord.dbd
#DBDINC += xxxRecord

# Install xxxSupport.dbd into <top>/dbd
#DBD += xxxSupport.dbd

# Build an IOC support library
#LIBRARY_IOC += motorHomeSupport

# Compile and add the code to the support library
#motorHomeSupport_SRCS += xxxRecord.c

# Link locally-provided code into the support library,
# rather than directly into the IOC application.
# This is required for Windows DLL builds.
#motorHomeSupport_SRCS += dbSubExample.c

motorHomeSupport_LIBS += $(EPICS_BASE_IOC_LIBS)

include $(TOP)/configure/RULES
#----------------------------------------
#  ADD EXTRA GNUMAKE RULES BELOW HERE

