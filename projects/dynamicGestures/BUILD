# Copyright 2020 Lisandro Bravo.
#
# Licensed under the Apache License, Version 2.0 (the "License");
# you may not use this file except in compliance with the License.
# You may obtain a copy of the License at
#
#      http://www.apache.org/licenses/LICENSE-2.0
#
# Unless required by applicable law or agreed to in writing, software
# distributed under the License is distributed on an "AS IS" BASIS,
# WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
# See the License for the specific language governing permissions and
# limitations under the License.

licenses(["notice"])  # Apache 2.0

package(default_visibility = ["//visibility:public"])

cc_library(
    name = "simple_run_graph_main",
    srcs = ["simple_run_graph_main.cc"],
    deps = [
        "//mediapipe/framework:calculator_framework",
        "//mediapipe/framework/port:commandlineflags",
        "//mediapipe/framework/port:file_helpers",
        "//mediapipe/framework/port:map_util",
        "//mediapipe/framework/port:parse_text_proto",
        "//mediapipe/framework/port:status",
        "@com_google_absl//absl/strings",
    ],
)

cc_library(
    name = "demo_run_graph_main",
    srcs = ["demo_run_graph_main.cc"],
    deps = [
        "//mediapipe/framework:calculator_framework",
        "//mediapipe/framework/formats:image_frame",
        "//mediapipe/framework/formats:image_frame_opencv",
        "//mediapipe/framework/port:commandlineflags",
        "//mediapipe/framework/port:file_helpers",
        "//mediapipe/framework/port:opencv_highgui",
        "//mediapipe/framework/port:opencv_imgproc",
        "//mediapipe/framework/port:opencv_video",
        "//mediapipe/framework/port:parse_text_proto",
        "//mediapipe/framework/port:status",
    ],
)

# Linux only.
# Must have a GPU with EGL support:
# ex: sudo apt-get install mesa-common-dev libegl1-mesa-dev libgles2-mesa-dev
# (or similar nvidia/amd equivalent)
cc_library(
    name = "demo_run_graph_main_gpu",
    srcs = ["demo_run_graph_main_gpu.cc"],
    deps = [
        "//mediapipe/framework:calculator_framework",
        "//mediapipe/framework/formats:image_frame",
        "//mediapipe/framework/formats:image_frame_opencv",
        "//mediapipe/framework/port:commandlineflags",
        "//mediapipe/framework/port:file_helpers",
        "//mediapipe/framework/port:opencv_highgui",
        "//mediapipe/framework/port:opencv_imgproc",
        "//mediapipe/framework/port:opencv_video",
        "//mediapipe/framework/port:parse_text_proto",
        "//mediapipe/framework/port:status",
        "//mediapipe/gpu:gl_calculator_helper",
        "//mediapipe/gpu:gpu_buffer",
        "//mediapipe/gpu:gpu_shared_data_internal",
    ],
)

#package(default_visibility = ["//mediapipe/examples:__subpackages__"])

# To use the 3D model instead of the default 2D model, add "--define 3D=true" to the
# bazel build command.
config_setting(
    name = "use_3d_model",
    define_values = {
        "3D": "true",
    },
)

genrule(
    name = "model",
    srcs = select({
        "//conditions:default": ["//mediapipe/models:hand_landmark.tflite"],
        ":use_3d_model": ["//mediapipe/models:hand_landmark_3d.tflite"],
    }),
    outs = ["hand_landmark.tflite"],
    cmd = "cp $< $@",
)

cc_binary(
    name = "dynamic_gestures_cpu_tflite",
    deps = [
        "simple_run_graph_main",
        "//myMediapipe/graphs/dynamicGestures:dynamic_gestures_desktop_cpu_calculators",
    ],
)

cc_binary(
    name = "dynamic_gestures_cpu_tflite_cam",
    deps = [
        "demo_run_graph_main",
        "//myMediapipe/graphs/dynamicGestures:dynamic_gestures_desktop_cpu_calculators",
    ],
)

cc_binary(
    name = "dynamic_gestures_gpu_tflite_cam",
    deps = [
        "demo_run_graph_main_gpu",
        "//myMediapipe/graphs/dynamicGestures:dynamic_gestures_desktop_cpu_calculators",
    ],
)
