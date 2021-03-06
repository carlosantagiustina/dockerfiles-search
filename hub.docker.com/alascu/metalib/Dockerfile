FROM alascu/metalib:base_isl_maint

# Pull repo
RUN \
    git clone --recursive https://github.com/PollyLabs/library-metamorphic-testing.git

# Set workdir to repo location
WORKDIR /home/library-metamorphic-testing

# Setup output folder and link
RUN mkdir /output && ln -s /output ./output

# Copy config file template over and update paths
RUN \
    sed 's|<work_dir>|'`pwd`'\/|' \
        < ./config_files/config.yaml.template \
        > ./config_files/config_isl_point.yaml && \
    sed -i 's|<lib>|isl_point|g' ./config_files/config_isl_point.yaml && \
    sed -i 's|lib_path:.*|lib_path: "/home/isl/.libs/"|' ./config_files/config_isl_point.yaml && \
    sed -i 's|include_path:.*|include_path: "/home/isl/include/"|' ./config_files/config_isl_point.yaml && \
    sed -i 's|lib_build_dir:.*|lib_build_dir: "/home/isl"|' ./config_files/config_isl_point.yaml && \
    sed -i 's|test_compile_bin:.*|test_compile_bin: "./compile_isl.sh"|' ./config_files/config_isl_point.yaml && \
    sed -i 's|default_timeout:.*|default_timeout: 120|' ./config_files/config_isl_point.yaml && \
    ln -s `pwd`/config_files/isl `pwd`/config_files/isl_point && \
    ln -s `pwd`/scripts/compile/compile_isl.sh `pwd`/out/compile_isl.sh && \
    ln -s `pwd`/config_files/isl/set_meta_tests_isl.yaml `pwd`/config_files/isl/set_meta_tests_isl_point.yaml && \
    curl -o ./include/isl-noexceptions.h www.doc.ic.ac.uk/~al2510/isl-noexceptions-noprint.h

ENV \
    CPATH="/home/isl/include" \
    LIBRARY_PATH="/home/isl/.libs" \
    LD_LIBRARY_PATH="/home/isl/.libs" \
    BUILD_SHARED_LIBS=true \
    YAML_BUILD_SHARED_LIBS=ON

# Build project
RUN mkdir build && cd build && cmake .. && make -j4

# Set command to run experiments
CMD ["python3.7", "./scripts/meta_runner.py", "--config-file", "./config_files/config_isl_point.yaml", "continuous", "--append-id"]
