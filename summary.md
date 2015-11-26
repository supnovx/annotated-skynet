
## Message Sources
- skynet_error(ctx, msg, ...)
- skynet_send(ctx, source, dest, type, session, data, sz)
- skynet_sendname(ctx, source, addr, type, session, data, sz)
- thread_timer(para) -> skynet_updatetime()
- thread_socket(para) -> skynet_socket_poll()
- skynet.timeout(time, func) -> skynet_timeout(handle, time, session)
- skynet.sleep(time) -> skynet_timeout(handle, time, session)

skynet_mq_push(queue, message)  
 <- skynet_context_push(handle, message)  
     <- skynet_error(ctx, msg, ...)  
     <- skynet_send(ctx, source, dest, type, session, data, sz)  
     <- forward_message(type, padding, socket_message) <- skynet_socket_poll() <- thread_socket(para)  
     <- dispatch_list(timer_node) <- timer_execute(timer) <- timer_update(timer) <- skynet_updatetime() <- thread_timer(para)  
     <- skynet_timeout(handle, time, session) <- cmd_timeout(ctx, para) <- skynet.timeout(time, func) skynet.sleep(time)  
 <- skynet_context_send(ctx, msg, sz, source, type, session)  
     <- skynet_harbor_send(remote_message, source, session)  
         <- skynet_send(ctx, source, dest, type, session, data, sz)  
         <- skynet_sendname(ctx, source, addr, type, session, data, sz)    

## Message Dispatch

- bootstrap() -> skynet_context_dispatchall(ctx)    
    => ctx->cb(ctx, ctx->cb_ud, type, msg->session, msg->source, msg->data, sz)
- thread_worker(para) -> skynet_context_message_dispatch(monitor, queue, weight)    
    => ctx->cb(ctx, ctx->cb_ud, type, msg->session, msg->source, msg->data, sz)
    