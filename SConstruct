# -*- python -*-

import os

# Define a recursive install builder, to be used later.
def RecursiveInstall(env, target, src):
    root_items = []
    for src_list in src:
        root_items.extend(src_list)

    for root_item in root_items:
        unsearched = [root_item]
        base_dir = os.path.split(str(root_item))[0]
        #Recursive search
        while unsearched:
            current = unsearched.pop()
            if current.isdir():
                unsearched.extend(Glob(str(current)+'/*'))
            else:
                dir_initial, filename = os.path.split(str(current))
                dir_relative = dir_initial[len(str(base_dir))+1:]
                output_dir = (os.path.join(str(target),dir_relative)
                              if dir_relative else str(target))
                env.Install(output_dir,current)
def TOOL_RECURSIVE_INSTALL(env):
    env.AddMethod(RecursiveInstall)

env = Environment(tools=['default',
                         TOOL_RECURSIVE_INSTALL],
                  ENV=os.environ)

env['CXXCOMSTR'] = 'Compiling object $TARGETS'
env['CCCOMSTR'] = 'Compiling object $TARGETS'
env['ARCOMSTR'] = 'Packing static library $TARGETS'
env['RANLIBCOMSTR'] = 'Indexing static library $TARGETS'
env['SHCXXCOMSTR'] = 'Compiling shared object $TARGETS'
env['LINKCOMSTR'] = 'Linking $TARGETS'

win32 = env.Clone()
win64 = env.Clone()
linux = env.Clone()

#Define the working directory
win32['SYS'] = 'win32'
win64['SYS'] = 'win64'
linux['SYS'] = 'linux'

#Define the compilers, using C++11
win32.Replace(CC='i686-w64-mingw32-gcc')
win32.Replace(CXX='i686-w64-mingw32-g++')
win32.Append(CXXFLAGS=['-std=c++0x'])
win64.Replace(CC='x86_64-w64-mingw32-gcc')
win64.Replace(CXX='x86_64-w64-mingw32-g++')
win64.Append(CXXFLAGS=['-std=c++0x'])
linux.Append(CXXFLAGS=['-std=c++11'])

#Define the appropriate file formats
win32.Replace(SHLIBPREFIX='')
win32.Replace(SHLIBSUFFIX='.dll')
win32.Replace(PROGSUFFIX='.exe')
win32.Append(LINKFLAGS='-static')
win64.Replace(SHLIBPREFIX='')
win64.Replace(SHLIBSUFFIX='.dll')
win64.Replace(PROGSUFFIX='.exe')
win64.Append(LINKFLAGS='-static')

#For building platform-independent resources
resources = SConscript('resources/SConscript')

for env in [win32,win64,linux]:
    env['ENV']['CC'] = env['CC']
    env['ENV']['CXX'] = env['CXX']
    build_dir = os.path.join('build',env['SYS'])
    exe = SConscript('SConscript',
                     variant_dir=build_dir,
                     src_dir='.',
                     exports=['env'])
    inst_dir = env['SYS']
    env.RecursiveInstall(inst_dir,[exe,resources])
    Clean('.',env['SYS'])

Clean('.','build')

