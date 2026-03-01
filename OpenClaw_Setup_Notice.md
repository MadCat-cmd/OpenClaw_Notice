
## How to setup the local lan dashboard

After the installation of Openclaw, the dasboard Web UI can only be accessed by local machine. That mean the Web UI 
gate is bind by `127.0.0.1`. If you want to access the Web UI dashboard from other deivces in same lan, some configuration 
should be changed. 

The configuration file is a json file, location is in following path:
```
/home/pi/.openclaw/openclaw.jason
```

The new openclaw require secure context to access the dashboard ui, that mean you the `https` singing is needed. If you
still want to use `http` access, that mean only use `<ip>:<port>` to access the dashboard in lan. the browser should runs in a 
a **non-secure** context and blocks WebCrypto. By default, OpenClaw **blocks** Control UI connections without device identity.

following configuration of `gateway` section should setup:

- set up the lan access, default value is `loopback`, which allow the dashboad can only be accessed by local machine
  ```
  openclaw config set gateway.bind lan
  ```

- tolggle the `Insecure-auth` behavior, set the `Insecure-auth` to true:
  ```
  openclaw config set gateway.controlUi.allowInsecureAuth true
  ```

- set `dangerouslyDisableDeviceAuth` to true,  disables Control UI device identity checks and is a severe security downgrade. Revert quickly after emergency use.

  ```
  openclaw config set gateway.controlUi.dangerouslyDisableDeviceAuth true
  ```

-  enables Host-header origin fallback mode, but is a dangerous security downgrade
  ```
  openclaw config set gateway.controlUi.dangerouslyAllowHostHeaderOriginFallback true
  ```

After those openclaw setup, you can check your new setup in configuration file `cpenclaw.json`. In the configuration file you should be able see those new section in `gateway` section:

```
    "controlUi": {
      "dangerouslyAllowHostHeaderOriginFallback": true,
      "allowInsecureAuth": true,
      "dangerouslyDisableDeviceAuth": true
    },

```

you can also get those configuration value via following command:

```
# get dangerouslyAllowHostHeaderOriginFallback value
openclaw config get gateway.controlUi.dangerouslyAllowHostHeaderOriginFallback

# get allowInsecureAuth value
openclaw config get gateway.controlUi.allowInsecureAuth

# get dangerouslyDisableDeviceAuth value
openclaw config get gateway.controlUi.dangerouslyDisableDeviceAuth
```


After the setup has been finished, you can connect the dashboard in local lan. Use following command to get the openclaw status:

```
openclaw status
```

you can see the the dasboard adress in the table, you can also use following command to check the dashbaord adress and the token:

```
openclaw dashboard --no-open
```


type this adress in your browser, you should see the dashboard content. But if you want to interactive with the dashboard, you should use the token 
to login. Go to dashboard overview, type your token in `Gateway Token` section, and click connect. Your token can be find in `config,json` file. or 
you can use this command to get the token：

```
openclaw config get gateway.auth.dangerouslyDisableDeviceAuth.token
```

##　Openclaw architecture

`Workspace`, `Agent` and `Session` form the openclaw running architecture. The full architecture look like following:

```
Workspace
   ├── Agent A
   │      ├── Session 1
   │      ├── Session 2
   │
   ├── Agent B
   │      ├── Session 3
```

Here is a example, how can a openclaw deploment look like:
```
一个 Workspace
    ├── Agent A（客服机器人）
    │       ├── Telegram Session 1
    │       ├── Telegram Session 2
    │       ├── WhatsApp Session 3
    │       ├── 飞书 Session 4
    │
    ├── Agent B（代码助手）
    │       ├── Telegram Session 5
```


### Workspace
Workspace is the complete running enviroment container, which contain follow:

- Agents
- Memory
- Tools
- Session
- Model
- Logs
- Vector Index
- Device Identity

### Agents
Agent is a `AI character`, which define following:
- Which AI Model is used
- System prompt (Which character/personality) setup
- Which tool can be used
- Which memory can be accessed
- Action policy

What does Angetn doesn't contain:
- chat history
- user history
those content is in session

Agent define following:
- Answering style
- which tool can be involked
- internet access
- long term memory allow 


### Session
Session is a chat instance, each session contain:
- independent chat context
- chat history
- short term state

For instance: the Telegram chatering agent chat with 2 human user:
```
User A -> Session A
User B -> Session B
```

What does Session folder save:
- Chat history
- current task state
- tempory context

### Memory
Memory is cross-session long term system

There are 2 kind of memory
- Short term Memory:
  exist in session, for continue chat session

- Long-term Memory:
  exist in workspace, which contain:
  - database
  - embeddings
  - user data
  - import fact


For instance:
When you in telegram say: 
>I am ...

Memory record this content, and you ask in feishu:
>who am I

The angent can always anwser it. 

### Tools
Tools define the function involking capability of agent, such as:
- command execution
- api involking
- database access
- read wirte file
- camera control

### Model
in workspace save:
- default Model
- Each Agent bind to a model
- model parameter

### Logs
Logs contain following:
- involking history
- tools execution history
- error messages
- running logs

### Vector Index
Vecotr Index is for:
- RAG
- semantic search
- long term memory searching index


### Device Identity
Device Identity is for:
- Web UI authentication
- Multi-Device authentication
- encryption sign























