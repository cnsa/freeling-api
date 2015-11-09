
# Change these paths to match your installation
# Alternatively, you can call 'make' overriding variable values, E.g.:
#     make FREELINGDIR=/my/freeling/dir SWIGDIR=/my/swig/dir JAVADIR=/my/java/dir

FREELING_OUT ?= /usr/local/lib/freeling
FREELINGDIR ?= /usr/local
SWIGDIR ?= /usr/share/swig2.0
JAVADIR ?= /usr/lib/jvm/java

# you may change the package name if you want
PACKAGE = edu/upc/freeling
JPACKAGE = edu.upc.freeling

# Change your compiler, if necessary.
GCC=g++

osx: freeling_javaAPI.cxx libfreeling_javaAPI.dylib

all: freeling_javaAPI.cxx libfreeling_javaAPI.so

.PHONY: all osx

# Compile the C++ API
libfreeling_javaAPI.dylib: freeling_javaAPI.cxx
	$(GCC) -dynamiclib $(FREELINGDIR)/lib/libfreeling.dylib -o $(FREELING_OUT)/libfreeling_javaAPI.dylib freeling_javaAPI.cxx -lfreeling -L$(FREELINGDIR)/lib -lboost_system -I/usr/local/opt/icu4c/include -I$(FREELINGDIR)/include -I$(JAVADIR)/include -I$(JAVADIR)/include/darwin -fPIC -std=c++0x
	$(JAVADIR)/bin/javac $(PACKAGE)/*.java
	$(JAVADIR)/bin/jar -cf $(FREELING_OUT)/freeling.jar $(PACKAGE)

libfreeling_javaAPI.so: freeling_javaAPI.cxx
	$(GCC) -shared -o $(FREELING_OUT)/libfreeling_javaAPI.so freeling_javaAPI.cxx -lfreeling -L$(FREELINGDIR)/lib -lboost_system -I$(FREELINGDIR)/include -I$(JAVADIR)/include -I$(JAVADIR)/include/linux -fPIC -std=c++0x
	$(JAVADIR)/bin/javac $(PACKAGE)/*.java
	$(JAVADIR)/bin/jar -cf $(FREELING_OUT)/freeling.jar $(PACKAGE)

# Build the Java JNI wrapper classes.
freeling_javaAPI.cxx: freeling_javaAPI.i ../common/freeling.i
	rm -rf $(PACKAGE)
	mkdir -p $(PACKAGE)
	mkdir -p $(FREELING_OUT)
	swig -java -c++ -package $(JPACKAGE) -outdir $(PACKAGE) -o freeling_javaAPI.cxx -I$(SWIGDIR)/java -I$(SWIGDIR)/std -I$(SWIGDIR) freeling_javaAPI.i

clean:
	rm -rf $(FREELING_OUT)/*.jar $(FREELING_OUT)/*.so $(FREELING_OUT)/*.dylib *.cxx *.class edu
