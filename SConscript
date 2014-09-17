# -*- python -*-

import os

Import('env')

env.Append(CPPPATH=['#/include'])
exe = env.Program(['Program.cc',Glob('src/*.cc')])

Return('exe')
