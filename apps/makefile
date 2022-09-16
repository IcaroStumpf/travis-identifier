# ==========================================
#   Unity Project - A Test Framework for C
#   Copyright (c) 2007 Mike Karlesky, Mark VanderVoord, Greg Williams
#   [Released under MIT License. Please refer to license.txt for details]
# ==========================================

#We try to detect the OS we are running on, and adjust commands as needed
ifeq ($(OS),Windows_NT)
  ifeq ($(shell uname -s),) # not in a bash-like shell
	CLEANUP = del /F /Q
	MKDIR = mkdir
  else # in a bash-like shell, like msys
	CLEANUP = rm -f
	MKDIR = mkdir -p
  endif
	TARGET_EXTENSION=.exe
else
	CLEANUP = rm -f
	MKDIR = mkdir -p
	TARGET_EXTENSION=.out
endif

C_COMPILER=gcc
CLANG_COMPILER=clang

UNITY_ROOT=..

CFLAGS=-std=c99
CFLAGS += -Wall
CFLAGS += -Wextra
CFLAGS += -Wpointer-arith
CFLAGS += -Wcast-align
CFLAGS += -Wwrite-strings
CFLAGS += -Wswitch-default
CFLAGS += -Wunreachable-code
CFLAGS += -Winit-self
CFLAGS += -Wmissing-field-initializers
CFLAGS += -Wno-unknown-pragmas
CFLAGS += -Wstrict-prototypes
CFLAGS += -Wundef
CFLAGS += -Wold-style-definition

TARGET_BASE1=all_tests1
TARGET_BASE2=all_tests2
TARGET_BASE3=all_tests3
TARGET_BASE4=all_tests4
TARGET_BASE5=all_tests5
TARGET1 = $(TARGET_BASE1)$(TARGET_EXTENSION)
TARGET2 = $(TARGET_BASE2)$(TARGET_EXTENSION)
TARGET3 = $(TARGET_BASE3)$(TARGET_EXTENSION)
TARGET4 = $(TARGET_BASE4)$(TARGET_EXTENSION)
TARGET5 = $(TARGET_BASE5)$(TARGET_EXTENSION)
SRC_FILES1=\
  $(UNITY_ROOT)/src/unity.c \
  $(UNITY_ROOT)/extras/fixture/src/unity_fixture.c \
  src/foo.c \
  test/TestFoo.c \
  test/test_runners/TestFoo_Runner.c \
  test/test_runners/all_tests.c
INC_DIRS=-Isrc -I$(UNITY_ROOT)/src -I$(UNITY_ROOT)/extras/fixture/src

all: clean compile run

compile:
	$(C_COMPILER) $(CFLAGS) $(INC_DIRS) $(SRC_FILES1) -o $(TARGET1)

sanitize:
	$(CLANG_COMPILER) $(CFLAGS) -fsanitize=address $(INC_DIRS) $(SRC_FILES1) -o $(TARGET2)
	./$(TARGET2) -v

cov:
	$(C_COMPILER) $(CFLAGS) -fprofile-arcs -ftest-coverage $(INC_DIRS) $(SRC_FILES1) -o $(TARGET3)
	./$(TARGET3) -v
	gcov -b foo.c
	
valgrind:
	gcc -g -Wall -Wfatal-errors $(INC_DIRS) $(SRC_FILES1) -o $(TARGET4)
	valgrind --leak-check=full --show-leak-kinds=all ./$(TARGET4) -v 
	
cppcheck: 
	cppcheck --enable=all --suppress=missingIncludeSystem $(SRC_FILES1) -o $(TARGET5)

run:
	./$(TARGET1) -v

clean:
	$(CLEANUP) $(TARGET1)
	$(CLEANUP) $(TARGET2)
	$(CLEANUP) $(TARGET3)
	$(CLEANUP) $(TARGET4)
	$(CLEANUP) $(TARGET5)
