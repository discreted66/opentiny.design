### 第一步：安装NEXT-SDKs

安装依赖包

```bash
npm install @opentiny/next-sdk
```

### 创建 WebMcpClient ，并与 WebAgent 连接

在 Web 应用的的主入口（比如：Vue 项目的 App.vue 文件）定义 WebMcpClient

```typescript
import { WebMcpClient } from '@opentiny/next-sdk'

// 创建 MCP Client
const client = new WebMcpClient({
  name: 'my-app-client',
  version: '1.0.0'
})

// 连接 Client Transport
await client.connect(clientTransport)

// 连接到 WebAgent 服务
const { sessionId } = await client.connect({
  agent: true,
  url: 'https://agent.opentiny.design/api/v1/webmcp-trial/mcp'
})

// 这个sessionId是Web 应用与 WebAgent 服务建立连接后，由 WebAgent 服务生成的用来唯一标识被操控的 Web 应用（被控端）
```

### 创建 WebMcpServer， 并与 serverTransport 连接

在Web应用的子页面（比如： views/page1.vue）中定义 WebMcpServer，每个页面可以定义自己的 WebMcpServer，页面切换时，MCP Client 会与当前页面的 MCP Server 建立连接，并丢弃与之前页面的连接。

```typescript
import { WebMcpServer, createMessageChannelPairTransport, z } from '@opentiny/next-sdk'

// 创建通信通道
const [serverTransport, clientTransport] = createMessageChannelPairTransport()

// 创建 MCP Server
const server = new WebMcpServer({
  name: 'my-app-server',
  version: '1.0.0'
})

// 注册工具
server.registerTool('demo-tool', {
  title: '演示工具',
  description: '这是一个演示工具',
  inputSchema: { 
    foo: z.string().describe('输入参数') 
  },
}, async (params) => {
  console.log('接收到参数:', params)
  return { 
    content: [{ 
      type: 'text', 
      text: `已处理: ${params.foo}` 
    }] 
  }
})

// 连接 Server Transport
await server.connect(serverTransport)
```



### 引入 TinyRemoter 遥控器组件，就这样完成

安装遥控器组件

```bash
npm install @opentiny/next-remoter
```
