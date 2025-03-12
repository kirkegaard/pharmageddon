# A simple Makefile for compiling small SDL projects

# set the compiler
CC ?= cc

DEMO_DIR = ../demo
ENGINE_DIR = ../engine

# Platform detection
UNAME_S := $(shell uname -s)
UNAME_M := $(shell uname -m)

# Default Assimp path 
ASSIMP_PATH ?= /usr
ifeq ($(UNAME_S), Darwin) # macOS
    ifeq ($(UNAME_M), arm64) # Apple Silicon
        ASSIMP_PATH := /opt/homebrew/opt/assimp
    else # Intel mac
        ASSIMP_PATH := /usr/local/opt/assimp
    endif
endif

# Flags for SDL2 and SDL2_mixer 
SDL_CFLAGS := $(shell pkg-config --cflags sdl2 SDL2_mixer)
SDL_LDFLAGS := $(shell pkg-config --libs sdl2 SDL2_mixer)

# Assimp include and link flags
ASSIMP_CFLAGS := -I$(ASSIMP_PATH)/include
ASSIMP_LDFLAGS := -L$(ASSIMP_PATH)/lib -lassimp

# flags
CFLAGS := $(SDL_CFLAGS) $(ASSIMP_CFLAGS) -ggdb3 -O0 --std=c99 -Wall -I $(DEMO_DIR) -I $(ENGINE_DIR)
LDFLAGS := $(SDL_LDFLAGS) $(ASSIMP_LDFLAGS) -lm

# add header files here
HDRS := $(DEMO_DIR)/demo.h

# add source files here
SRCS := main.c

# generate names of object files
OBJS := $(SRCS:.c=.o) $(DEMO_DIR)/demo.a $(ENGINE_DIR)/engine.a

# name of executable
EXEC := pharma

# default recipe
all: $(EXEC)

$(DEMO_DIR)/demo.a: FORCE
	$(MAKE) -C $(DEMO_DIR) CFLAGS="$(CFLAGS)" LDFLAGS="$(LDFLAGS)"

$(ENGINE_DIR)/engine.a: FORCE
	$(MAKE) -C $(ENGINE_DIR) CFLAGS="$(CFLAGS)" LDFLAGS="$(LDFLAGS)"

$(EXEC): $(OBJS) $(HDRS) Makefile
	$(CC) -o $@ $(OBJS) $(CFLAGS) $(LDFLAGS)

clean:
	rm -f $(EXEC) $(SRCS:.c=.o)

.PHONY: all clean FORCE
