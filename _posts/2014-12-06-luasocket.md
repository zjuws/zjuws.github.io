---
layout: post
title: "mac下安装LuaSocket"
description: "lua"
category: lua
tags: [lua,socket]
---
{% include JB/setup %}

##安装LuaSocket
LuaSocket 是 Lua 的网络模块库，它可以很方便地提供 TCP、UDP、DNS、FTP、HTTP、SMTP、MIME 等多种网络协议的访问操作。它由两部分组成：一部分是用 C 写的核心，提供对 TCP 和 UDP 传输层的访问支持。另外一部分是用 Lua 写的，负责应用功能的网络接口处理。

###安装LuaSocket


* Homebrew安装（如果已经安装略过此步）


首先你要安装Homebrew。安装 Homebrew 很简单，只需在终端上输入一行 Ruby 脚本（所以要先搭建 Ruby 运行环境，Mac 下已经预装了 Ruby）就行：

	ruby -e "$(curl -fsSL https://raw.github.com/Homebrew/homebrew/go/install)"

* LuaRocks安装 (如果已经安装略过此步) 

Mac 下的 Homebrew 居然内置了 LuaRocks 的安装包（之前安装 Lua，用 "brew search lua" 搜 Lua 安装包时无意间发现），因此，在 Mac 下安装 LuaRocks 很简单，一条指令就行：

	brew install luarocks -v


* LuaSocket安装

如果你安装有 Lua 模块的安装和部署工具 -- LuaRocks，那么一条指令就能安装部署好 LuaSocket：


	luarocks install luasocket


###LuaSocket 使用

使用 LuaSocket 很简单，直接用 require 函数加载进来就行，例如输出一个 LuaSocket 版本信息：
	
	local socket = require("socket")
	print(socket._VERSION)
模块 LuaSocket 内置的常量、函数的结构图如下：

	- sleep [function: 0x7feeeb40f940]
	- source [function: 0x7feeeb413570]
	- newtry [function: 0x7feeeb40f8c0]
	- _VERSION [LuaSocket 2.0.2]
	- connect [function: 0x7feeeb4122f0]
	- sink [function: 0x7feeeb410ea0]
	- __unload [function: 0x7feeeb4107e0]
	- bind [function: 0x7feeeb413380]
	- _M {.}
	- _DEBUG [true]
	- skip [function: 0x7feeeb4107b0]
	- dns - gethostname [function: 0x7feeeb410af0]
	|     - tohostname [function: 0x7feeeb410b20]
	|     - toip [function: 0x7feeeb410aa0]
	- gettime [function: 0x7feeeb40f8f0]
	- select [function: 0x7feeeb412290]
	- BLOCKSIZE [2048]
	- sinkt - default [function: 0x7feeeb410e20]
	|       - close-when-done [function: 0x7feeeb410dc0]
	|       - keep-open [function: 0x7feeeb410e20]
	- sourcet - by-length [function: 0x7feeeb410e50]
	|         - default [function: 0x7feeeb413440]
	|         - until-closed [function: 0x7feeeb413440]
	- tcp [function: 0x7feeeb412020]
	- _NAME [socket]
	- choose [function: 0x7feeeb410ce0]
	- try [function: 0x7feeeb410ca0]
	- protect [function: 0x7feeeb410760]
	- _PACKAGE []
	- udp [function: 0x7feeeb410fd0]
	
以 socket 的方式访问获取度娘首页数据：

	local socket = require("socket")
	 
	local host = "www.baidu.com"
	local file = "/"
	 
	-- 创建一个 TCP 连接，连接到 HTTP 连接的标准端口 -- 80 端口上
	local sock = assert(socket.connect(host, 80))
	sock:send("GET " .. file .. " HTTP/1.0\r\n\r\n")
	repeat
	    -- 以 1K 的字节块来接收数据，并把接收到字节块输出来
	    local chunk, status, partial = sock:receive(1024)
	    print(chunk or partial)
	until status ~= "closed"
	-- 关闭 TCP 连接
	sock:close()
	
或者使用模块里内置的 http 方法来访问：

	local http = require("socket.http")
	local response = http.request("http://www.baidu.com/")
	print(response)

以下是一个简单的http方法的例子，用于某学校的网络连接。

	local http=require("socket.http");

    local request_body = [[username=21451141&password=...]]
    local response_body = {}

    local res, code, response_headers = http.request{
        url = "http://192.0.0.6/cgi-bin/do_login",
        method = "POST",
        headers =
          {
              ["Content-Type"] = "application/x-www-form-urlencoded";
              ["Content-Length"] = #request_body;
          },
          source = ltn12.source.string(request_body),
          sink = ltn12.sink.table(response_body),
    }

    print(res)
    print(code)

    if type(response_headers) == "table" then
      for k, v in pairs(response_headers) do
        print(k, v)
      end
    end

    print("Response body:")
    if type(response_body) == "table" then
      print(table.concat(response_body))
    else
      print("Not a table:", type(response_body))
    end
    
###一个简单的 client/server 通信连接

在客户端的终端上敲一些东西后回车会通过 socket 给服务器发送数据，服务器接收到数据后再返回显示在客户端的终端上。一个简单的东西，纯属练手，代码如下：

	-- server.lua
	local socket = require("socket")
	 
	local host = "127.0.0.1"
	local port = "12345"
	local server = assert(socket.bind(host, port, 1024))
	server:settimeout(0)
	local client_tab = {}
	local conn_count = 0
	 
	print("Server Start " .. host .. ":" .. port) 
	 
	while 1 do
	    local conn = server:accept()
	    if conn then
	        conn_count = conn_count + 1
	        client_tab[conn_count] = conn
	        print("A client successfully connect!") 
	    end
	  
	    for conn_count, client in pairs(client_tab) do
	        local recvt, sendt, status = socket.select({client}, nil, 1)
	        if #recvt > 0 then
	            local receive, receive_status = client:receive()
	            if receive_status ~= "closed" then
	                if receive then
	                    assert(client:send("Client " .. conn_count .. " Send : "))
	                    assert(client:send(receive .. "\n"))
	                    print("Receive Client " .. conn_count .. " : ", receive)   
	                end
	            else
	                table.remove(client_tab, conn_count) 
	                client:close() 
	                print("Client " .. conn_count .. " disconnect!") 
	            end
	        end
	         
	    end
	end

客户端：
	
	-- client.lua
	local socket = require("socket")
	 
	local host = "127.0.0.1"
	local port = 12345
	local sock = assert(socket.connect(host, port))
	sock:settimeout(0)
	  
	print("Press enter after input something:")
	 
	local input, recvt, sendt, status
	while true do
	    input = io.read()
	    if #input > 0 then
	        assert(sock:send(input .. "\n"))
	    end
	     
	    recvt, sendt, status = socket.select({sock}, nil, 1)
	    while #recvt > 0 do
	        local response, receive_status = sock:receive()
	        if receive_status ~= "closed" then
	            if response then
	                print(response)
	                recvt, sendt, status = socket.select({sock}, nil, 1)
	            end
	        else
	            break
	        end
	    end
	end
	
感谢[D.H.Q的烂笔头](http://dhq.me/luasocket-network-lua-module)