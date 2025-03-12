# A simple Makefile for compiling small SDL projects on macOS (Apple Silicon)

# set the compiler
CC := clang

DEMO_DIR = ../demo
ENGINE_DIR = ../engine

ASSIMP_PATH := /opt/homebrew/opt/assimp

CFLAGS := $(shell pkg-config --cflags sdl2 SDL2_mixer) -I$(ASSIMP_PATH)/include -ggdb3 -O0 --std=c99 -Wall -I $(DEMO_DIR) -I $(ENGINE_DIR)
LDFLAGS := $(shell pkg-config --libs sdl2 SDL2_mixer) -L$(ASSIMP_PATH)/lib -lassimp -lm

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
	$(MAKE) -C $(DEMO_DIR) CFLAGS="$(CFLAGS)"


$(ENGINE_DIR)/engine.a: FORCE
	$(MAKE) -C $(ENGINE_DIR) CFLAGS="$(CFLAGS)"

# recipe for building the final executable
$(EXEC): $(OBJS) $(HDRS) Makefile
	$(CC) -o $@ $(OBJS) $(CFLAGS) $(LDFLAGS)

# recipe to clean the workspace
clean:
	rm -f $(EXEC) $(OBJS)

.PHONY: all clean FORCE
