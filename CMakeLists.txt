# Copyright (c) 2024 Sparse Technology, Mustafa Abdullah Kus
# SPDX-License-Identifier: Apache-2.0

if(CONFIG_ZEROMQ)
    add_library(ZEROMQ INTERFACE)

    add_compile_definitions(__ZEPHYR__)

    set(cxx-sources
        precompiled.cpp
        address.cpp
        channel.cpp
        client.cpp
        clock.cpp
        ctx.cpp
        curve_mechanism_base.cpp
        curve_client.cpp
        curve_server.cpp
        dealer.cpp
        devpoll.cpp
        dgram.cpp
        dist.cpp
        endpoint.cpp
        epoll.cpp
        err.cpp
        fq.cpp
        io_object.cpp
        io_thread.cpp
        ip.cpp
        ipc_address.cpp
        ipc_connecter.cpp
        ipc_listener.cpp
        kqueue.cpp
        lb.cpp
        mailbox.cpp
        mailbox_safe.cpp
        mechanism.cpp
        mechanism_base.cpp
        metadata.cpp
        msg.cpp
        mtrie.cpp
        norm_engine.cpp
        object.cpp
        options.cpp
        own.cpp
        null_mechanism.cpp
        pair.cpp
        peer.cpp
        pgm_receiver.cpp
        pgm_sender.cpp
        pgm_socket.cpp
        pipe.cpp
        plain_client.cpp
        plain_server.cpp
        poll.cpp
        poller_base.cpp
        polling_util.cpp
        pollset.cpp
        proxy.cpp
        pub.cpp
        pull.cpp
        push.cpp
        random.cpp
        raw_encoder.cpp
        raw_decoder.cpp
        raw_engine.cpp
        reaper.cpp
        rep.cpp
        req.cpp
        router.cpp
        select.cpp
        server.cpp
        session_base.cpp
        signaler.cpp
        socket_base.cpp
        socks.cpp
        socks_connecter.cpp
        stream.cpp
        stream_engine_base.cpp
        sub.cpp
        tcp.cpp
        tcp_address.cpp
        tcp_connecter.cpp
        tcp_listener.cpp
        thread.cpp
        trie.cpp
        radix_tree.cpp
        v1_decoder.cpp
        v1_encoder.cpp
        v2_decoder.cpp
        v2_encoder.cpp
        v3_1_encoder.cpp
        xpub.cpp
        xsub.cpp
        zmq.cpp
        zmq_utils.cpp
        decoder_allocators.cpp
        socket_poller.cpp
        timers.cpp
        config.hpp
        radio.cpp
        dish.cpp
        udp_engine.cpp
        udp_address.cpp
        scatter.cpp
        gather.cpp
        ip_resolver.cpp
        zap_client.cpp
        zmtp_engine.cpp

        # at least for VS, the header files must also be listed
        address.hpp
        array.hpp
        atomic_counter.hpp
        atomic_ptr.hpp
        blob.hpp
        channel.hpp
        client.hpp
        clock.hpp
        command.hpp
        compat.hpp
        condition_variable.hpp
        config.hpp
        ctx.hpp
        curve_client.hpp
        curve_client_tools.hpp
        curve_mechanism_base.hpp
        curve_server.hpp
        dbuffer.hpp
        dealer.hpp
        decoder.hpp
        decoder_allocators.hpp
        devpoll.hpp
        dgram.hpp
        dish.hpp
        dist.hpp
        encoder.hpp
        endpoint.hpp
        epoll.hpp
        err.hpp
        fd.hpp
        fq.hpp
        gather.hpp
        generic_mtrie.hpp
        generic_mtrie_impl.hpp
        gssapi_client.hpp
        gssapi_mechanism_base.hpp
        gssapi_server.hpp
        i_decoder.hpp
        i_encoder.hpp
        i_engine.hpp
        i_mailbox.hpp
        i_poll_events.hpp
        io_object.hpp
        io_thread.hpp
        ip.hpp
        ipc_address.hpp
        ipc_connecter.hpp
        ipc_listener.hpp
        kqueue.hpp
        lb.hpp
        likely.hpp
        macros.hpp
        mailbox.hpp
        mailbox_safe.hpp
        mechanism.hpp
        mechanism_base.hpp
        metadata.hpp
        msg.hpp
        mtrie.hpp
        mutex.hpp
        norm_engine.hpp
        null_mechanism.hpp
        object.hpp
        options.hpp
        own.hpp
        pair.hpp
        peer.hpp
        pgm_receiver.hpp
        pgm_sender.hpp
        pgm_socket.hpp
        pipe.hpp
        plain_client.hpp
        plain_common.hpp
        plain_server.hpp
        poll.hpp
        poller.hpp
        poller_base.hpp
        polling_util.hpp
        pollset.hpp
        precompiled.hpp
        proxy.hpp
        pub.hpp
        pull.hpp
        push.hpp
        radio.hpp
        random.hpp
        raw_decoder.hpp
        raw_encoder.hpp
        raw_engine.hpp
        reaper.hpp
        rep.hpp
        req.hpp
        router.hpp
        scatter.hpp
        secure_allocator.hpp
        select.hpp
        server.hpp
        session_base.hpp
        signaler.hpp
        socket_base.hpp
        socket_poller.hpp
        socks.hpp
        socks_connecter.hpp
        stdint.hpp
        stream.hpp
        stream_engine_base.hpp
        stream_connecter_base.hpp
        stream_connecter_base.cpp
        stream_listener_base.hpp
        stream_listener_base.cpp
        sub.hpp
        tcp.hpp
        tcp_address.hpp
        tcp_connecter.hpp
        tcp_listener.hpp
        thread.hpp
        timers.hpp
        tipc_address.hpp
        tipc_connecter.hpp
        tipc_listener.hpp
        trie.hpp
        udp_address.hpp
        udp_engine.hpp
        v1_decoder.hpp
        v1_encoder.hpp
        v2_decoder.hpp
        v2_encoder.hpp
        v3_1_encoder.hpp
        v2_protocol.hpp
        vmci.hpp
        vmci_address.hpp
        vmci_connecter.hpp
        vmci_listener.hpp
        windows.hpp
        wire.hpp
        xpub.hpp
        xsub.hpp
        ypipe.hpp
        ypipe_base.hpp
        ypipe_conflate.hpp
        yqueue.hpp
        zap_client.hpp
        zmtp_engine.hpp)

    target_include_directories(ZEROMQ INTERFACE
        ${ZEPHYR_CURRENT_MODULE_DIR}/libzmq/src/
    )

    target_link_libraries(ZEROMQ INTERFACE zephyr_interface)


    include(CheckFunctionExists)
    include(CheckIncludeFile)
    include(CheckIncludeFiles)
    include(CheckSymbolExists)
    include(CheckStructHasMember)
    include(CheckCSourceCompiles)
    include(CheckCXXSourceCompiles)

    check_include_file(netinet/in.h HAVE_NETINET_IN_H)
    check_include_file(sys/socket.h HAVE_SYS_SOCKET_H)

    find_package(Threads)

    # TO DO: connect kconfig select, epoll and poll
    set(POLLER "poll")
    set(API_POLLER "poll")

    message(STATUS "Using polling method in zmq_poll(er)_* API: ${API_POLLER}")
    string(TOUPPER ${API_POLLER} UPPER_API_POLLER)
    set(ZMQ_POLL_BASED_ON_${UPPER_API_POLLER} 1)

    message(STATUS "Using polling method in I/O threads: ${POLLER}")
    string(TOUPPER ${POLLER} UPPER_POLLER)
    set(ZMQ_IOTHREAD_POLLER_USE_${UPPER_POLLER} 1)

    set(ZMQ_CV_IMPL_DEFAULT "pthreads")

    set(ZMQ_USE_CV_IMPL_PTHREADS 1)

    foreach(source ${cxx-sources})
        list(APPEND sources ${ZEPHYR_CURRENT_MODULE_DIR}/libzmq/src/${source})
    endforeach()

    configure_file(${ZEPHYR_CURRENT_MODULE_DIR}/libzmq/builds/cmake/platform.hpp.in ${CMAKE_CURRENT_BINARY_DIR}/generated/platform.hpp)

    zephyr_library()

    zephyr_library_sources(
        ${sources}
    )

    zephyr_include_directories(${CMAKE_CURRENT_BINARY_DIR}/generated)
    zephyr_include_directories(${ZEPHYR_CURRENT_MODULE_DIR}/libzmq/src)
    zephyr_include_directories(${ZEPHYR_CURRENT_MODULE_DIR}/libzmq/include)

    zephyr_library_link_libraries(ZEROMQ)

    # zephyr_library_compile_options(-Wno-builtin-macro-redefined)
endif()
