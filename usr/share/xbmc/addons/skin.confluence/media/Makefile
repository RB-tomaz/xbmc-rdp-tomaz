TARGET=Textures.xbt
XBMCTEX=/home/pi/xbmc-rbp/tools/TexturePacker/TexturePacker

IMAGES := $(shell find . -name "*jpg")
IMAGES += $(shell find . -name "*png")
IMAGES += $(shell find . -name "*gif")

all: $(TARGET)

.PHONY: force

$(TARGET): $(XBMCTEX) $(IMAGES)
	$(XBMCTEX) -input . -output $(TARGET)

$(XBMCTEX): force
	$(MAKE) -C /home/pi/xbmc-rbp/tools/TexturePacker

clean:
	rm -f $(TARGET)
distclean: clean

