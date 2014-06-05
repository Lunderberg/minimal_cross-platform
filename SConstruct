# -*- python -*-

import os
from os.path import join as pjoin

win32 = Environment()
win32.Replace(CC='i686-w64-mingw32-gcc')
win32.Append(CCFLAGS=['-std=c++0x'])
win32.Replace(CXX='i686-w64-mingw32-g++')
win32.Replace(SHLIBPREFIX='')
win32.Replace(SHLIBSUFFIX='.dll')
win32.Replace(PROGSUFFIX='.exe')
win32.Append(LINKFLAGS='-static')
win32.dir = 'build_win32'
win32.inst_dir = 'bin_win32'

win64 = Environment()
win64.Replace(CC='x86_64-w64-mingw32-gcc')
win64.Append(CCFLAGS=['-std=c++0x'])
win64.Replace(CXX='x86_64-w64-mingw32-g++')
win64.Replace(SHLIBPREFIX='')
win64.Replace(SHLIBSUFFIX='.dll')
win64.Replace(PROGSUFFIX='.exe')
win64.Append(LINKFLAGS='-static')
win64.dir = 'build_win64'
win64.inst_dir = 'bin_win64'

native = Environment()
native.Append(CCFLAGS=['-std=c++11'])
native.dir = 'build'
native.inst_dir = 'bin'


def compile_all(env):
    env.Append(CPPPATH=['include'])

    env.VariantDir(env.dir,'.',duplicate=0)
    exe = env.Program([pjoin(env.dir,'Program.cc'),
                       Glob(pjoin(env.dir,'src','*.cc'))])
    env.Install(env.inst_dir,exe)

    env.Clean('.',env.dir)
    env.Clean('.',env.inst_dir)


for env in [win32,win64,native]:
    compile_all(env)

