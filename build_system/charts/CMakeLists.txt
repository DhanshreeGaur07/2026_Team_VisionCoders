add_custom_command(
    OUTPUT ${CMAKE_INSTALL_PREFIX}/verifirewall-k8s-nginx-ingress-4.1.4.tgz
    COMMAND helm package ${CMAKE_SOURCE_DIR}/build_system/charts/verifirewall-k8s-nginx-ingress && mv ${CMAKE_SOURCE_DIR}/build_system/charts/verifirewall-k8s-nginx-ingress-4.1.4.tgz ${CMAKE_INSTALL_PREFIX}/verifirewall-k8s-nginx-ingress-4.1.4.tgz
)

add_custom_target(charts DEPENDS ${CMAKE_INSTALL_PREFIX}/verifirewall-k8s-nginx-ingress-4.1.4.tgz)
