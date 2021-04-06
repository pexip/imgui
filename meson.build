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
gl_dep = dependency('GL')
glfw3_dep = dependency('glfw3', static : true)

if gl_dep.found() and glfw3_dep.found()
  glfw3_sources = [
    'examples/libs/gl3w/GL/gl3w.c',
  ]

  glfw3_includes = include_directories('examples/libs/gl3w')
  libglfw3 = static_library('imgui_glfw',
    files(glfw3_sources),
    dependencies : [gl_dep, glfw3_dep],
    include_directories : glfw3_includes,
    install : true,
  )

  libglfw3_dep = declare_dependency(
    link_with : libglfw3,
    include_directories : glfw3_includes,
  )

  # ImGui GLFW Backend
  imgui_glfw_backend_sources = [
    'backends/imgui_impl_glfw.cpp',
    'backends/imgui_impl_opengl3.cpp',
  ]

  libimgui_glfw_backend = static_library('imgui_glfw_backend',
    files(imgui_glfw_backend_sources),
    dependencies : [libglfw3_dep, gl_dep, glfw3_dep],
    include_directories : glfw3_includes,
    install : true,
  )

  libimgui_glfw_backend_dep = declare_dependency(
    link_with : libimgui_glfw_backend,
    include_directories : include_directories('backends'),
  )
endif
