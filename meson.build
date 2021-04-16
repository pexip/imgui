host_system = host_machine.system()
cc = meson.get_compiler('c')

gl_dep = dependency('gl', required : false)

# ImGui Core
imgui_sources = [
  'imgui.cpp',
  'imgui_draw.cpp',
  'imgui_tables.cpp',
  'imgui_widgets.cpp',

  # Misc/cpp
  'misc/cpp/imgui_stdlib.h',
  'misc/cpp/imgui_stdlib.cpp',
]

libimgui_core = static_library('imgui_core',
  files(imgui_sources),
  install: true,
)

libimgui_core_dep = declare_dependency(
  link_with : libimgui_core,
  include_directories : include_directories('.'),
)

# GLFW3 lib
if host_system == 'windows'
  glfw3_dep = cc.find_library('glfw3',
    required : true,
    dirs : meson.current_source_dir() + '/examples/libs/glfw/lib-vc2017',
  )
else
  glfw3_dep = dependency('glfw3', static : true)
endif

if gl_dep.found() and glfw3_dep.found()
  common_deps = []
  glfw_includes = []
  if host_system == 'windows'
    common_deps = [
      cc.find_library('gdi32'),
      cc.find_library('shell32'),
      cc.find_library('imm32'),
    ]

    glfw_includes += [ include_directories('examples/libs/glfw/include') ]
  endif

  gl3w_sources = [
    'examples/libs/gl3w/GL/gl3w.c',
  ]

  gl3w_includes = [ include_directories('examples/libs/gl3w') ]
  libglfw3 = static_library('imgui_glfw',
    files(gl3w_sources),
    dependencies : [gl_dep, glfw3_dep] + common_deps,
    include_directories : gl3w_includes,
    install : true,
  )

  libglfw3_dep = declare_dependency(
    link_with : libglfw3,
    include_directories : gl3w_includes,
  )

  # ImGui GLFW Backend
  imgui_glfw_backend_sources = [
    'backends/imgui_impl_glfw.cpp',
    'backends/imgui_impl_opengl3.cpp',
  ]

  libimgui_glfw_backend = static_library('imgui_glfw_backend',
    files(imgui_glfw_backend_sources),
    dependencies : [libglfw3_dep, gl_dep, glfw3_dep],
    include_directories : gl3w_includes + glfw_includes,
    install : true,
  )

  libimgui_glfw_backend_dep = declare_dependency(
    link_with : libimgui_glfw_backend,
    include_directories : [ include_directories('backends') ] + glfw_includes,
  )
endif