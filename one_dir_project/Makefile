#
# (C) Copyleft 2011 2012 2013 2014 2015 2016
# Late Lee from http://www.latelee.org
# 
# A simple Makefile for *ONE* project(c or/and cpp file) in *ONE* or *MORE* directory
#
# note: 
# you can put head file(s) in 'include' directory, so it looks 
# a little neat.
#
# usage: $ make
#        $ make CROSS_COMPILE=arm-arago-linux-gnueabi-
#        $ make debug=y
#        $ make SRC_DIR1=foo SRC_DIR2=bar SRC_DIR3=crc
#
# log
#       2013-05-14 sth about debug...
#       2016-02-29 sth for c/c++ multi diretory
###############################################################################

# !!!=== cross compile...
CROSS_COMPILE = 

CC  = $(CROSS_COMPILE)gcc
CXX = $(CROSS_COMPILE)g++
AR  = $(CROSS_COMPILE)ar

ARFLAGS = -cr
RM     = -rm -rf
MAKE   = make

# !!!===
# target executable file or .a or .so
target = a.out

# !!!===
# compile flags
CFLAGS := -Wall -Wfatal-errors

#****************************************************************************
# debug can be set to y to include debugging info, or n otherwise
debug  := y

#****************************************************************************

ifeq ($(debug), y)
CFLAGS += -ggdb -rdynamic
else
    # if target is lib(.a/.so), don't need "-O2 -s"
    # == if (target != .a && target != .so)
    ifneq ($(suffix $(target)), .a)
    ifneq ($(suffix $(target)), .so)
    CFLAGS += -O2 -s
    endif
    endif
endif

# !!!===
DEFS    = -DFUCK

CFLAGS  += $(DEFS)

LIBS    := 

LDFLAGS := $(LIBS)

# !!!===
INC1 = ./
INC2 = ./inc
INC3 = 
INCDIRS := -I$(INC1) -I$(INC2)

# !!!===
CFLAGS += $(INCDIRS)

# !!!===
LDFLAGS += -lpthread -lrt

DYNC_FLAGS += -fpic -shared

# !!!===
# source file(s), including c file(s) or cpp file(s)
# you can also use $(wildcard *.c), etc.
SRC_DIR = .
SRC_DIR1 = 
SRC_DIR2 = 
SRC_DIR3 = 

# ok for c/c++
SRC = $(wildcard $(SRC_DIR)/*.c $(SRC_DIR)/*.cpp)
SRC+=$(wildcard $(SRC_DIR1)/*.c $(SRC_DIR1)/*.cpp)
SRC+=$(wildcard $(SRC_DIR2)/*.c $(SRC_DIR2)/*.cpp)
SRC+=$(wildcard $(SRC_DIR3)/*.c $(SRC_DIR3)/*.cpp)

# ok for c/c++
OBJ = $(patsubst %.c,%.o, $(patsubst %.cpp,%.o, $(SRC))) 


# !!!===
# in case all .c/.cpp need g++...
# CC = $(CXX)
###############################################################################

all: $(target)

$(target): $(OBJ)

ifeq ($(suffix $(target)), .so)
	@echo "Generating dynamic lib file..." $(notdir $(target))
	@$(CXX) $(CFLAGS) $^ -o $(target) $(LDFLAGS) $(DYNC_FLAGS)
else ifeq ($(suffix $(target)), .a)
	@echo "Generating static lib file..." $(notdir $(target))
	@$(AR) $(ARFLAGS) -o $(target) $^
else
	@echo "Generating executable file..." $(notdir $(target))
	@$(CXX) $(CFLAGS) $^ -o $(target) $(LDFLAGS)
endif

# make all .c or .cpp
%.o: %.c
	@echo "Compiling: " $(addsuffix .c, $(basename $(notdir $@)))
	@$(CC) $(CFLAGS) -c $< -o $@

%.o: %.cpp
	@echo "Compiling: " $(addsuffix .cpp, $(basename $(notdir $@)))
	@$(CXX) $(CFLAGS) -c $< -o $@

clean:
	@echo "Cleaning..."
	@$(RM) $(target)

# use 'grep -v soapC.o' to skip the file
	@find . -iname '*.o' -o -iname '*.bak' -o -iname '*.d' | xargs rm -f

.PHONY: all clean
