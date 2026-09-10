add_executable(algebra_determinants main.cpp)
target_compile_features(algebra_determinants PRIVATE cxx_std_20)
target_compile_definitions(algebra_determinants PRIVATE
    ALGEBRA_ASSIGNMENT_SIZE=${ALGEBRA_ASSIGNMENT_SIZE}
)
target_link_libraries(algebra_determinants PRIVATE
    algebra::finite_fields
    algebra::matrices
)
