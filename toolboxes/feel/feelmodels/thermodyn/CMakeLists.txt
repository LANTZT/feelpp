
# geo order 1
foreach(_DIM 2 3)
  math(EXPR _T_ORDER '${FEELPP_INSTANTIATION_ORDER_MAX}-1')
  feelpp_max( MAX_ORDER 1 ${_T_ORDER})
  message(STATUS "[feelpp] THERMODYN ${_DIM}D PNG1 N=1..${MAX_ORDER}")
  foreach( _T_ORDER RANGE 1 ${MAX_ORDER} )
    genLibThermoDynamics(
      DIM     ${_DIM}
      T_ORDER ${_T_ORDER}
      GEO_ORDER 1
      ADD_CMAKE_INSTALL
      )
    set(FEELPP_MODELS_LIBRARIES_THERMODYNAMICS  ${FEELPP_MODELS_LIBRARIES_THERMODYNAMICS} ${LIBBASE_NAME}  )
  endforeach()
endforeach()

# isoparametric
foreach(_DIM 2 3)
  math(EXPR _T_ORDER '${FEELPP_INSTANTIATION_ORDER_MAX}-1')
  feelpp_min( MAX_ORDER ${FEELPP_MESH_MAX_ORDER} ${_T_ORDER})
  message(STATUS "[feelpp] THERMODYN ${_DIM}D PNGN N=2..${MAX_ORDER}")
  foreach( _T_ORDER RANGE 2 ${MAX_ORDER} )
    genLibThermoDynamics(
      DIM     ${_DIM}
      T_ORDER ${_T_ORDER}
      GEO_ORDER ${_T_ORDER}
      ADD_CMAKE_INSTALL
      )
    set(FEELPP_MODELS_LIBRARIES_THERMODYNAMICS  ${FEELPP_MODELS_LIBRARIES_THERMODYNAMICS} ${LIBBASE_NAME} )
  endforeach()
endforeach()

add_library(feelpp_model_thermodynamics SHARED libthermodynamics.cpp )
target_link_libraries(feelpp_model_thermodynamics ${FEELPP_MODELS_LIBRARIES_THERMODYNAMICS})
install(TARGETS feelpp_model_thermodynamics ${FEELPP_MODELS_LIBRARIES_THERMODYNAMICS} DESTINATION lib COMPONENT Lib)
