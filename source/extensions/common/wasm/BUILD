licenses(["notice"])  # Apache 2

load(
    "//bazel:envoy_build_system.bzl",
    "envoy_cc_library",
    "envoy_package",
    "envoy_proto_library",
)

envoy_package()

envoy_cc_library(
    name = "well_known_names",
    hdrs = ["well_known_names.h"],
    deps = [
        "//source/common/singleton:const_singleton",
    ],
)

# NB: Used to break the circular dependency between wasm_lib and null_plugin_lib.
envoy_cc_library(
    name = "wasm_hdr",
    hdrs = [
        "context.h",
        "foreign.h",
        "wasm.h",
        "wasm_state.h",
        "wasm_vm.h",
    ],
    deps = [
        ":well_known_names",
        "//include/envoy/http:codes_interface",
        "//include/envoy/http:filter_interface",
        "//include/envoy/server:wasm_interface",
        "//include/envoy/upstream:cluster_manager_interface",
        "//source/common/config:datasource_lib",
        "//source/common/singleton:const_singleton",
        "//source/common/stats:stats_lib",
        "//source/extensions/filters/common/expr:evaluator_lib",
        "//source/extensions/filters/http:well_known_names",
        "@com_google_cel_cpp//eval/public:activation",
        "@envoy_api//envoy/config/wasm/v3:pkg_cc_proto",
        "@proxy_wasm_cpp_host//:include",
        "@proxy_wasm_cpp_sdk//:common_lib",
    ],
)

envoy_cc_library(
    name = "wasm_interoperation_lib",
    srcs = [
        "wasm_state.cc",
    ],
    hdrs = [
        "wasm_state.h",
    ],
    deps = [
        "//include/envoy/stream_info:filter_state_interface",
        "//source/common/protobuf",
        "//source/common/singleton:const_singleton",
        "@com_github_google_flatbuffers//:flatbuffers",
        "@com_google_cel_cpp//eval/public:cel_value",
        "@com_google_cel_cpp//tools:flatbuffers_backed_impl",
    ],
)

envoy_cc_library(
    name = "wasm_lib",
    srcs = [
        "context.cc",
        "foreign.cc",
        "wasm.cc",
        "wasm_vm.cc",
    ],
    deps = [
        ":declare_property_cc_proto",
        ":wasm_hdr",
        ":wasm_interoperation_lib",
        "//external:abseil_base",
        "//external:abseil_node_hash_map",
        "//include/envoy/server:lifecycle_notifier_interface",
        "//source/common/buffer:buffer_lib",
        "//source/common/common:enum_to_int",
        "//source/common/http:message_lib",
        "//source/common/http:utility_lib",
        "//source/common/tracing:http_tracer_lib",
        "//source/extensions/filters/common/expr:context_lib",
        "@com_google_cel_cpp//eval/eval:field_access",
        "@com_google_cel_cpp//eval/eval:field_backed_list_impl",
        "@com_google_cel_cpp//eval/eval:field_backed_map_impl",
        "@com_google_cel_cpp//eval/public:builtin_func_registrar",
        "@com_google_cel_cpp//eval/public:cel_expr_builder_factory",
        "@com_google_cel_cpp//eval/public:cel_value",
        "@com_google_cel_cpp//eval/public:value_export_util",
        "@com_google_cel_cpp//parser",
        "@envoy_api//envoy/config/wasm/v3:pkg_cc_proto",
        "@proxy_wasm_cpp_host//:lib",
    ],
)

envoy_proto_library(
    name = "declare_property",
    srcs = ["declare_property.proto"],
)
