set(flb_plugins "" CACHE INTERNAL "flb_plugins")

# REGISTER_CUSTOM_PLUGIN
macro(REGISTER_CUSTOM_PLUGIN name)
  string(FIND ${name} "=" pos)
  if(pos GREATER -1)
    string(REPLACE "=" ";" list ${name})
    list(GET list 0 p_name)
    list(GET list 1 p_path)
    message(STATUS "EXTERNAL CUSTOM PLUGIN name='${p_name}' path='${p_path}'")
  else()
    set(p_name ${name})
  endif()

  string(TOUPPER ${p_name} NAME)
  if(FLB_${NAME} OR p_path)
    set(FLB_IN_PLUGINS_DECL "${FLB_CUSTOM_PLUGINS_DECL}extern struct flb_custom_plugin ${p_name}_plugin;\n")

    # C code
    set(C_CODE          "    custom = flb_malloc(sizeof(struct flb_custom_plugin));\n")
    set(C_CODE "${C_CODE}    if (!custom) {\n")
    set(C_CODE "${C_CODE}        flb_errno();\n")
    set(C_CODE "${C_CODE}        return -1;\n")
    set(C_CODE "${C_CODE}    }\n")
    set(C_CODE "${C_CODE}    memcpy(custom, &${p_name}_plugin, sizeof(struct flb_custom_plugin));\n")
    set(C_CODE "${C_CODE}    mk_list_add(&custom->_head, &config->custom_plugins);\n\n")

    set(FLB_IN_PLUGINS_ADD "${FLB_CUSTOM_PLUGINS_ADD}${C_CODE}")

    if (p_path)
      add_subdirectory(${p_path} ${p_path})
    else()
      add_subdirectory(${p_name})
    endif()
    set(flb_plugins "${flb_plugins}flb-plugin-${p_name};")
  endif()
endmacro()

# REGISTER_IN_PLUGIN
macro(REGISTER_IN_PLUGIN name)
  string(FIND ${name} "=" pos)
  if(pos GREATER -1)
    string(REPLACE "=" ";" list ${name})
    list(GET list 0 p_name)
    list(GET list 1 p_path)
    message(STATUS "EXTERNAL IN PLUGIN name='${p_name}' path='${p_path}'")
  else()
    set(p_name ${name})
  endif()

  string(TOUPPER ${p_name} NAME)
  if(FLB_${NAME} OR p_path)
    set(FLB_IN_PLUGINS_DECL "${FLB_IN_PLUGINS_DECL}extern struct flb_input_plugin ${p_name}_plugin;\n")

    # C code
    set(C_CODE          "    in = flb_malloc(sizeof(struct flb_input_plugin));\n")
    set(C_CODE "${C_CODE}    if (!in) {\n")
    set(C_CODE "${C_CODE}        flb_errno();\n")
    set(C_CODE "${C_CODE}        return -1;\n")
    set(C_CODE "${C_CODE}    }\n")
    set(C_CODE "${C_CODE}    memcpy(in, &${p_name}_plugin, sizeof(struct flb_input_plugin));\n")
    set(C_CODE "${C_CODE}    mk_list_add(&in->_head, &config->in_plugins);\n\n")

    set(FLB_IN_PLUGINS_ADD "${FLB_IN_PLUGINS_ADD}${C_CODE}")

    if (p_path)
      add_subdirectory(${p_path} ${p_path})
    else()
      add_subdirectory(${p_name})
    endif()
    set(flb_plugins "${flb_plugins}flb-plugin-${p_name};")
  endif()
endmacro()

# REGISTER_OUT_PLUGIN
macro(REGISTER_OUT_PLUGIN name)
  string(FIND ${name} "=" pos)
  if(pos GREATER -1)
    string(REPLACE "=" ";" list ${name})
    list(GET list 0 p_name)
    list(GET list 1 p_path)
    message(STATUS "EXTERNAL OUT PLUGIN name='${p_name}' path='${p_path}'")
  else()
    set(p_name ${name})
  endif()

  string(TOUPPER ${p_name} NAME)
  if(FLB_${NAME} OR p_path)
    set(FLB_OUT_PLUGINS_DECL "${FLB_OUT_PLUGINS_DECL}extern struct flb_output_plugin ${p_name}_plugin;\n")

    # C code
    set(C_CODE          "    out = flb_malloc(sizeof(struct flb_output_plugin));\n")
    set(C_CODE "${C_CODE}    if (!out) {\n")
    set(C_CODE "${C_CODE}        flb_errno();\n")
    set(C_CODE "${C_CODE}        return -1;\n")
    set(C_CODE "${C_CODE}    }\n")
    set(C_CODE "${C_CODE}    memcpy(out, &${p_name}_plugin, sizeof(struct flb_output_plugin));\n")
    set(C_CODE "${C_CODE}    mk_list_add(&out->_head, &config->out_plugins);\n\n")

    set(FLB_OUT_PLUGINS_ADD "${FLB_OUT_PLUGINS_ADD}${C_CODE}")
    if (p_path)
      add_subdirectory(${p_path} ${p_path})
    else()
      add_subdirectory(${p_name})
    endif()
    set(flb_plugins "${flb_plugins}flb-plugin-${p_name};")
  endif()
endmacro()

# REGISTER_FILTER_PLUGIN
macro(REGISTER_FILTER_PLUGIN name)
  string(FIND ${name} "=" pos)
  if(pos GREATER -1)
    string(REPLACE "=" ";" list ${name})
    list(GET list 0 p_name)
    list(GET list 1 p_path)
    message(STATUS "EXTERNAL FILTER PLUGIN name='${p_name}' path='${p_path}'")
  else()
    set(p_name ${name})
  endif()

  string(TOUPPER ${p_name} NAME)
  if(FLB_${NAME} OR p_path)
    set(FLB_FILTER_PLUGINS_DECL "${FLB_FILTER_PLUGINS_DECL}extern struct flb_filter_plugin ${p_name}_plugin;\n")

    # C code
    set(C_CODE          "    filter = flb_malloc(sizeof(struct flb_filter_plugin));\n")
    set(C_CODE "${C_CODE}    if (!filter) {\n")
    set(C_CODE "${C_CODE}        flb_errno();\n")
    set(C_CODE "${C_CODE}        return -1;\n")
    set(C_CODE "${C_CODE}    }\n")
    set(C_CODE "${C_CODE}    memcpy(filter, &${p_name}_plugin, sizeof(struct flb_filter_plugin));\n")
    set(C_CODE "${C_CODE}    mk_list_add(&filter->_head, &config->filter_plugins);\n\n")

    set(FLB_FILTER_PLUGINS_ADD "${FLB_FILTER_PLUGINS_ADD}${C_CODE}")
    if (p_path)
      add_subdirectory(${p_path} ${p_path})
    else()
      add_subdirectory(${p_name})
    endif()
    set(flb_plugins "${flb_plugins}flb-plugin-${p_name};")
  endif()
endmacro()

# FLB_PLUGIN: used by plugins to perform registration and linking
macro(FLB_PLUGIN name src deps)
  add_library(flb-plugin-${name} STATIC ${src})
  add_sanitizers(flb-plugin-${name})
  target_link_libraries(flb-plugin-${name} fluent-bit-static msgpack-c-static ${deps})
endmacro()


# ======================
#  Plugins Registration
# ======================

# INPUTS
# =======
REGISTER_IN_PLUGIN("in_collectd")
REGISTER_IN_PLUGIN("in_dummy")
REGISTER_IN_PLUGIN("in_emitter")
REGISTER_IN_PLUGIN("in_forward")
REGISTER_IN_PLUGIN("in_lib")
REGISTER_IN_PLUGIN("in_opentelemetry")

if (FLB_IN_STORAGE_BACKLOG)
  REGISTER_IN_PLUGIN("in_storage_backlog")
endif()


# OUTPUTS
# =======
REGISTER_OUT_PLUGIN("out_es")
REGISTER_OUT_PLUGIN("out_lib")
REGISTER_OUT_PLUGIN("out_null")
REGISTER_OUT_PLUGIN("out_opentelemetry")
REGISTER_OUT_PLUGIN("out_retry")
REGISTER_OUT_PLUGIN("out_stdout")


# FILTERS
# =======
REGISTER_FILTER_PLUGIN("filter_aws")

if(FLB_REGEX)
  REGISTER_FILTER_PLUGIN("filter_kubernetes")
  REGISTER_FILTER_PLUGIN("filter_multiline")
  REGISTER_FILTER_PLUGIN("filter_parser")
endif()

if(FLB_RECORD_ACCESSOR)
  REGISTER_FILTER_PLUGIN("filter_rewrite_tag")
endif()

if(FLB_LUAJIT)
  REGISTER_FILTER_PLUGIN("filter_lua")
endif()

# Register external input and output plugins
if(EXT_IN_PLUGINS)
  string(REPLACE "," ";" plugins ${EXT_IN_PLUGINS})
  foreach(entry ${plugins})
    REGISTER_IN_PLUGIN(${entry})
  endforeach()
endif()

if(EXT_OUT_PLUGINS)
  string(REPLACE "," ";" plugins ${EXT_OUT_PLUGINS})
  foreach(entry ${plugins})
    REGISTER_OUT_PLUGIN(${entry})
  endforeach()
endif()

if(EXT_FILTER_PLUGINS)
  string(REPLACE "," ";" plugins ${EXT_FILTER_PLUGINS})
  foreach(entry ${plugins})
    REGISTER_FILTER_PLUGIN(${entry})
  endforeach()
endif()

# Generate the header from the template
configure_file(
  "${PROJECT_SOURCE_DIR}/include/fluent-bit/flb_plugins.h.in"
  "${PROJECT_SOURCE_DIR}/include/fluent-bit/flb_plugins.h"
  )

set(FLB_PLUGINS "${flb_plugins}" PARENT_SCOPE)
