import os
import platform
from SCons.Script.Main import GetOption
from SCons.Script.Main import AddOption
from SCons.Script import SConscript
from SCons.Defaults import DefaultEnvironment

# Get the Default Build Environment
default_env = DefaultEnvironment()

# Create Build Environment
temp_env = default_env.Environment()
if platform.system().lower().startswith('win'):
    temp_env.PrependENVPath(
        "PATH", r'M:\cm\Common\texlive\v2.0.0\bin\win32')
elif platform.system().lower().startswith('linux'):
    temp_env.PrependENVPath(
        "PATH", '/nfs/msdev/cm/Common/texlive/v2.0.0/bin/x86_64-linux')
elif platform.system().lower().startswith('darwin'):
    temp_env.PrependENVPath(
        "PATH", '/nfs/msdev/cm/Common/texlive/v2.0.0/bin/universal-darwin')
env = default_env.Environment(tools=['default', 'pdflatex'], ENV=temp_env['ENV'])
common = '../Common'
env.PrependENVPath('PATH', common)
env.Append(PDFLATEXFLAGS=['-halt-on-error', '-shell-escape', '-synctex=1'],
           TEXINPUTS=[common, os.path.join(common, 'media')])

# Program Options
AddOption('--synctex', dest='synctex', action='store_true', default=False,
          help="enable build mode complient with most latex ide's")

# Build
default_env.Export('env')
kwargs = {}
if not GetOption('synctex'):
    kwargs['variant_dir'] = 'build'
    kwargs['duplicate'] = 'y'
doc = SConscript('src/SConscript', **kwargs)
default_env.Install('dist', doc)
