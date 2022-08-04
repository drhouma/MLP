
PRO_FILENAME=MLP
INSTALL_DIR=application
TECH_SUPPORT_DIR=$(INSTALL_DIR)/tech_support
TAR_NAME=MLP.tar
SRC=model/*.cpp controller/*.cpp view/*.cpp matrix/*.cpp
HDR=model/*.h controller/*.h view/*.h

all: dist install dvi tests

.PHONY: build dist all install uninstall

install: $(INSTALL_DIR) build
ifeq ($(shell uname),Darwin)
	-@cp -r ./build/$(PRO_FILENAME).app $(INSTALL_DIR)/
	@rm -rf $(PRO_FILENAME).app
else
	-@cp -r ./build/$(PRO_FILENAME) $(INSTALL_DIR)/
	@rm -rf $(PRO_FILENAME)
endif
	@make clean

uninstall:
ifeq ($(shell uname),Darwin)
	@rm -rf $(INSTALL_DIR)/MLP.app
else
	@rm -rf $(INSTALL_DIR)/MLP
endif

build: createBuild createTempMakefile
	@make -C ./build

clean: 
	@-rm -rf build *.out

distclean: clean
	@-rm -rf $(INSTALL_DIR)

createTempMakefile:
	cmake -B./build ./

run:
ifeq ($(shell uname),Darwin)
	@-./$(INSTALL_DIR)/MLP.app/Contents/MacOS/MLP
else
	@-./$(INSTALL_DIR)/MLP
endif

dist: $(INSTALL_DIR)
	@tar -cvf $(INSTALL_DIR)/$(TAR_NAME) ./
	@gzip $(INSTALL_DIR)/$(TAR_NAME)

dvi: $(INSTALL_DIR)/$(TECH_SUPPORT_DIR)
ifeq ($(shell uname),Darwin)
	@open https://github.com/sav1nbrave4code/MLP/blob/master/README_RUS.md
else
	@xdg-open https://github.com/sav1nbrave4code/MLP/blob/master/README_RUS.md
endif

$(INSTALL_DIR)/$(TECH_SUPPORT_DIR):
	@-mkdir $(TECH_SUPPORT_DIR)

$(INSTALL_DIR):
	-mkdir $(INSTALL_DIR)

createBuild:
	-mkdir build

linter:
	cp ../materials/linters/CPPLINT.cfg ./
	-@python3 ../materials/linters/cpplint.py --filter=-build/include_subdir model/*.cpp controller/*.cpp matrix/*.cpp view/main_window.cpp view/main.cpp view/render_scene.cpp view/settings_window.cpp view/test_window.cpp view/train_window.cpp
	rm CPPLINT.cfg

cppcheck:
	@cppcheck --enable=all --language=c++ --suppress=unusedFunction --suppress=missingInclude model/*.cpp controller/*.cpp matrix/*.cpp view/main_window.cpp view/main.cpp view/render_scene.cpp view/settings_window.cpp view/test_window.cpp view/train_window.cpp $(HDR)

tests:
	@g++ model/testdir/main_test.cpp model/*.cpp matrix/*.cpp -o tests.out -lgtest
	@./tests.out
