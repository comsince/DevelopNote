# 融云SDK设计解析
## 一. 说明
   融云SDK主要包括两部分
 

 -  即时通讯协议
   这部分主要包括融云发起聊天的主要api，不过其核心的方法都封装在so库中，java层提供方法声明及回调，供外部使用
 - push协议部分
   主要包括push通道建立，push通知消息的下发

        
## 二.即时通讯协议部分

### 2.1.RongIMClient 获取通讯API
融云通讯利用service ipc的方式向RongClient提供即时通讯相关的api，下面的IHandler的声明方法
```
void connect(String var1, IStringCallback var2) throws RemoteException;

    void disconnect(boolean var1, IOperationCallback var2) throws RemoteException;

    void registerMessageType(String var1) throws RemoteException;

    int getTotalUnreadCount() throws RemoteException;

    int getUnreadCount(int[] var1) throws RemoteException;

    int getUnreadCountById(int var1, String var2) throws RemoteException;

    void setOnReceiveMessageListener(OnReceiveMessageListener var1) throws RemoteException;

    void setConnectionStatusListener(IConnectionStatusListener var1) throws RemoteException;

    Message insertMessage(Message var1, String var2) throws RemoteException;

    Message sendMessage(Message var1, String var2, ILongCallback var3) throws RemoteException;

    List<Message> getNewestMessages(Conversation var1, int var2) throws RemoteException;

    List<Message> getOlderMessages(Conversation var1, long var2, int var4) throws RemoteException;

    List<Message> getOlderMessagesByObjectName(Conversation var1, String var2, long var3, int var5) throws RemoteException;

    boolean deleteMessage(int[] var1) throws RemoteException;

    boolean clearMessages(Conversation var1) throws RemoteException;

    boolean clearMessagesUnreadStatus(Conversation var1) throws RemoteException;

    boolean setMessageExtra(int var1, String var2) throws RemoteException;

    boolean setMessageReceivedStatus(int var1, int var2) throws RemoteException;

    boolean setMessageSentStatus(int var1, int var2) throws RemoteException;

    List<Conversation> getConversationList() throws RemoteException;

    List<Conversation> getConversationListByType(int[] var1) throws RemoteException;

    Conversation getConversation(int var1, String var2) throws RemoteException;

    boolean removeConversation(int var1, String var2) throws RemoteException;

    boolean saveConversationDraft(Conversation var1, String var2) throws RemoteException;

    String getConversationDraft(Conversation var1) throws RemoteException;

    boolean cleanConversationDraft(Conversation var1) throws RemoteException;

    void getConversationNotificationStatus(int var1, String var2, ILongCallback var3) throws RemoteException;

    void setConversationNotificationStatus(int var1, String var2, int var3, ILongCallback var4) throws RemoteException;

    boolean setConversationTopStatus(int var1, String var2, boolean var3) throws RemoteException;

    int getConversationUnreadCount(Conversation var1) throws RemoteException;

    boolean clearConversations(int[] var1) throws RemoteException;

    void setConversationNotificationQuietHours(String var1, int var2, IOperationCallback var3) throws RemoteException;

    void removeConversationNotificationQuietHours(IOperationCallback var1) throws RemoteException;

    void getConversationNotificationQuietHours(IGetConversationNotificationQuietHoursCallback var1) throws RemoteException;

    void getDiscussion(String var1, IResultCallback var2) throws RemoteException;

    void setDiscussionName(String var1, String var2, IOperationCallback var3) throws RemoteException;

    void createDiscussion(String var1, List<String> var2, IResultCallback var3) throws RemoteException;

    void addMemberToDiscussion(String var1, List<String> var2, IOperationCallback var3) throws RemoteException;

    void removeDiscussionMember(String var1, String var2, IOperationCallback var3) throws RemoteException;

    void quitDiscussion(String var1, IOperationCallback var2) throws RemoteException;

    void syncGroup(List<Group> var1, IOperationCallback var2) throws RemoteException;

    void joinGroup(String var1, String var2, IOperationCallback var3) throws RemoteException;

    void quitGroup(String var1, IOperationCallback var2) throws RemoteException;

    void joinChatRoom(String var1, int var2, IOperationCallback var3) throws RemoteException;

    void quitChatRoom(String var1, IOperationCallback var2) throws RemoteException;

    void searchPublicService(String var1, int var2, int var3, IResultCallback var4) throws RemoteException;

    void subscribePublicService(String var1, int var2, boolean var3, IOperationCallback var4) throws RemoteException;

    void getPublicServiceInfo(String var1, int var2, IResultCallback var3) throws RemoteException;

    void getPublicServiceList(IResultCallback var1) throws RemoteException;

    boolean validateAuth(String var1) throws RemoteException;

    void uploadMedia(Conversation var1, String var2, IUploadCallback var3) throws RemoteException;

    void downloadMedia(Conversation var1, int var2, String var3, IDownloadMediaCallback var4) throws RemoteException;

    long getDeltaTime() throws RemoteException;

    void setDiscussionInviteStatus(String var1, int var2, IOperationCallback var3) throws RemoteException;

    void addToBlacklist(String var1, IOperationCallback var2) throws RemoteException;

    void removeFromBlacklist(String var1, IOperationCallback var2) throws RemoteException;

    String getTextMessageDraft(Conversation var1) throws RemoteException;

    boolean saveTextMessageDraft(Conversation var1, String var2) throws RemoteException;

    boolean clearTextMessageDraft(Conversation var1) throws RemoteException;

    void getBlacklist(IStringCallback var1) throws RemoteException;

    void getBlacklistStatus(String var1, IIntegerCallback var2) throws RemoteException;

    void setUserData(UserData var1, IOperationCallback var2) throws RemoteException;
```

> **Note:** IHandler原型 public interface IHandler extends IInterface {}

RongIMClient 在初始化的时候绑定RongService,代码如下：
```
public class RongService extends Service {
    LibHandlerStub mStub;
    private final String TAG = RongService.class.getSimpleName();

    public RongService() {
    }

    public void onCreate() {
        super.onCreate();
        Log.d(this.TAG, "onCreate, pid=" + Process.myPid());
        LibContext.init(this);
        Thread.setDefaultUncaughtExceptionHandler(new RongExceptionHandler(this));
    }

    public IBinder onBind(Intent intent) {
        Log.d(this.TAG, "onBind, pid=" + Process.myPid());
        this.mStub = new LibHandlerStub(LibContext.getInstance());
        this.mStub.init();
        return this.mStub;
    }

    public boolean onUnbind(Intent intent) {
        Log.d(this.TAG, "onUnbind, pid=" + Process.myPid());
        return false;
    }

    public void onDestroy() {
        Log.d(this.TAG, "onDestroy, pid=" + Process.myPid());
        Process.killProcess(Process.myPid());
    }
}

```
在RongClient绑定成功后，其可以可以使用IHandler提供的接口方法，下面是RongIMClient绑定service成功后的部分代码

```
//绑定RongService
Intent intent = new Intent(this.mContext, RongService.class);
this.mContext.bindService(intent, new RongIMClient.AidlConnection(), 1);
```

下面是ServiceConnection代码
```
class AidlConnection implements ServiceConnection {
        AidlConnection() {
        }

        public void onServiceConnected(ComponentName name, IBinder service) {
            RLog.d(RongIMClient.this, "AIDL", "onServiceConnected");
            RongIMClient.this.mLibHandler = io.rong.imlib.ipc.remote.IHandler.Stub.asInterface(service);
            RongIMClient.this.initConnectionStatus();
            RLog.d(this, "AIDL", RongIMClient.this.mLibHandler.toString());
            RongIMClient.this.mIsBind = true;
            RongIMClient.mHandler.post(RongIMClient.this.new RegRunnable());
            IntentFilter filter = new IntentFilter("android.net.conn.CONNECTIVITY_CHANGE");
            IntentFilter reconnectFilter = new IntentFilter("action_reconnect");
            RongIMClient.this.mContext.registerReceiver(RongIMClient.sS.mConnectChangeReceiver, filter);
            RongIMClient.this.mContext.registerReceiver(RongIMClient.sS.mConnectChangeReceiver, reconnectFilter);
            if(RongIMClient.this.mToken != null) {
                RongIMClient.this.reconnect((RongIMClient.ConnectCallback)null);
            }

            if(RongIMClient.this.mConnectRunable != null) {
                RongIMClient.mHandler.post(RongIMClient.this.mConnectRunable);
            }

        }

        public void onServiceDisconnected(ComponentName name) {
            RLog.d(RongIMClient.this, "AIDL", "onServiceDisconnected");
            RongIMClient.this.mLibHandler = null;
            RongIMClient.this.mIsBind = false;
            RongIMClient.this.mStatusListener.onStatusChange(RongIMClient.ConnectionStatusListener.ConnectionStatus.DISCONNECTED);
            RongIMClient.init(RongIMClient.this.mContext);
        }
    }
```

### 2.2.RongIM心跳设计
前面已经说明RongIMClient，初始化后就绑定了RongService，之后利用IHandler提供的接口调用底层API
这里要介绍一个调用nativeAPI的对象NativeClient，它主要负责将LibHandlerStub中的方法转化为调用nativeAPI，其部分声明如下：
```
protected native void setJNIEnv(NativeObject var1);

    protected native int InitClient(String var1, String var2, String var3, String var4, String var5);

    protected native void RegisterMessageType(String var1, int var2);

    protected native void Connect(String var1, NativeObject.ConnectAckCallback var2);

    protected native void Disconnect(int var1);

    protected native void SendFileWithUrl(String var1, int var2, int var3, byte[] var4, int var5, NativeObject.ProgressCallback var6);

    protected native void DownFileWithUrl(String var1, int var2, int var3, String var4, NativeObject.ProgressCallback var5);

    protected native byte[] GetPagedMessage(String var1, int var2, int var3, int var4);

    protected native NativeObject.Message[] GetPagedMessageEx(String var1, int var2, int var3, int var4);

    protected native byte[] SearchMessages(String var1, int var2, int var3);

    protected native NativeObject.Message[] SearchMessagesEx(String var1, int var2, int var3);

    protected native byte[] GetHistoryMessages(String var1, int var2, String var3, int var4, int var5);

    protected native boolean DeleteMessages(int[] var1);

    protected native boolean ClearMessages(int var1, String var2);

    protected native boolean ClearUnread(int var1, String var2);

    protected native boolean SetMessageExtra(int var1, String var2);

    protected native boolean RemoveConversation(int var1, String var2);

    protected native boolean SetTextMessageDraft(int var1, String var2, String var3);

    protected native boolean SetMessageContent(int var1, byte[] var2);

    protected native boolean SetMessageDisplayText(int var1, byte[] var2);

    protected native String GetTextMessageDraft(int var1, String var2);

    protected native byte[] GetRecentConversation();

    protected native boolean SetIsTop(int var1, String var2, boolean var3);

    protected native int GetTotalUnreadCount();

    protected native long GetDeltaTime();

    protected native void CreateInviteDiscussion(String var1, String[] var2, NativeObject.CreateDiscussionCallback var3);

    protected native void InviteMemberToDiscussion(String var1, String[] var2, NativeObject.PublishAckListener var3);

    protected native void RemoveMemberFromDiscussion(String var1, String var2, NativeObject.PublishAckListener var3);

    protected native void QuitDiscussion(String var1, NativeObject.PublishAckListener var2);

    protected native int SaveMessage(String var1, int var2, String var3, String var4, byte[] var5, byte[] var6);

    protected native void SendMessage(String var1, int var2, int var3, String var4, byte[] var5, byte[] var6, int var7, NativeObject.PublishAckListener var8);

    protected native void GetUserInfo(String var1, NativeObject.UserInfoOutputListener var2);

    protected native NativeObject.UserInfo GetUserInfoSync(String var1);

    protected native NativeObject.UserInfo GetUserInfoExSync(String var1, int var2);

    protected native void SetMessageListener(NativeObject.ReceiveMessageListener var1);

    protected native boolean SetReadStatus(int var1, int var2);

    protected native boolean SetSendStatus(int var1, int var2);

    protected native void SetWakeupQueryListener(NativeObject.WakeupQueryListener var1);

    protected native void EnvironmentChangeNotify(int var1, byte[] var2, int var3, NativeObject.EnvironmentChangeNotifyListener var4);

    protected native void GetDiscussionInfo(String var1, NativeObject.DiscussionInfoListener var2);

    protected native NativeObject.DiscussionInfo GetDiscussionInfoSync(String var1);

    protected native void SetExceptionListener(NativeObject.ExceptionListener var1);

    protected native void RenameDiscussion(String var1, String var2, NativeObject.PublishAckListener var3);

    protected native byte[] GetConversation(String var1, int var2);

    protected native void SetBlockPush(String var1, int var2, boolean var3, NativeObject.BizAckListener var4);

    protected native void GetBlockPush(String var1, int var2, NativeObject.BizAckListener var3);

    protected native int GetBlockPush(String var1, int var2);

    protected native void SetInviteStatus(String var1, int var2, NativeObject.PublishAckListener var3);

    protected native int GetUnreadCount(String var1, int var2);

    protected native byte[] GetConversationList(int[] var1);

    protected native NativeObject.Conversation[] GetConversationListEx(int[] var1);

    protected native void SyncGroups(String[] var1, String[] var2, NativeObject.PublishAckListener var3);

    protected native void JoinGroup(String var1, String var2, NativeObject.PublishAckListener var3);

    protected native void QuitGroup(String var1, String var2, NativeObject.PublishAckListener var3);

    protected native int GetCateUnreadCount(int[] var1);

    protected native void JoinChatRoom(String var1, int var2, int var3, NativeObject.PublishAckListener var4);

    protected native void QuitChatRoom(String var1, int var2, NativeObject.PublishAckListener var3);

    protected native boolean ClearConversations(int[] var1);

    protected native void AddToBlacklist(String var1, NativeObject.PublishAckListener var2);

    protected native void RemoveFromBlacklist(String var1, NativeObject.PublishAckListener var2);

    protected native void GetBlacklistStatus(String var1, NativeObject.BizAckListener var2);

    protected native void GetBlacklist(NativeObject.SetBlacklistListener var1);

    protected native void GetUploadToken(int var1, NativeObject.TokenListener var2);

    protected native void GetDownloadToken(int var1, String var2, NativeObject.TokenListener var3);

    protected native void SubscribeAccount(String var1, int var2, boolean var3, NativeObject.PublishAckListener var4);

    protected native void SetDeviceInfo(String var1, String var2, String var3, String var4, String var5);

    protected native void SearchAccount(String var1, int var2, int var3, NativeObject.AccountInfoListener var4);
```
> **Note:** 由于这些方法都打包在so库，具体的实现就不说明了

心跳的开始是基于连接服务器成功，即是执行connect()方法成功，下面的代码即是NativeClient中执行connect的核心代码：

```
public static NativeClient connect(String token, final NativeClient.IResultCallback<String> callback) throws Exception {
        if(nativeObj == null) {
            throw new RuntimeException("NativeClient 尚未初始化!");
        } else if(mVersion != null && resourcePath != null) {
            if(client == null) {
                client = new NativeClient();
            }

            if(token == null) {
                throw new IllegalArgumentException("Token不能为空。");
            } else {
                client.token = token;
                setEnvInfo(sContext);
                Log.d("NativeClient", "connect");
                nativeObj.Connect(token, new ConnectAckCallback() {
                    public void operationComplete(int status, String userId) {
                        if(status == 0) {
                        //注意这里连接成功后，就执行心跳逻辑
                            WakeLockUtils.startNextHeartbeat(NativeClient.sContext);
                            NativeClient.setConnectionStatusListener(NativeClient.mConnectStatusListener);
                            if(NativeClient.mConnectStatusListener != null) {
                                NativeClient.mConnectStatusListener.onChanged(status);
                            }

                            NativeClient.client.currentUserId = userId;
                            if(callback != null) {
                                callback.onSuccess(userId);
                            }
                        } else if(callback != null) {
                            callback.onError(status);
                        }

                    }
                });
                client.setOnReceiveMessageListener((NativeClient.OnReceiveMessageListener)null);
                return client;
            }
        } else {
            throw new IllegalArgumentException("AppKey或资源路径异常。");
        }
    }
```

下面的代码执行心跳逻辑代码，原理是采用定时器的方式来实现的
```
class WakeLockUtils {
    private static final int HEARTBEAT_SPAN = 180000;

    WakeLockUtils() {
    }

    static void startNextHeartbeat(Context context) {
        RLog.d(context, "startNextHeartbeat", context.getPackageName());
        Intent heartbeatIntent = new Intent(context, HeartbeatReceiver.class);
        heartbeatIntent.setPackage(context.getPackageName());
        PendingIntent intent = PendingIntent.getBroadcast(context, 0, heartbeatIntent, 0);
        AlarmManager alarmManager = (AlarmManager)context.getSystemService("alarm");
        long time = SystemClock.elapsedRealtime() + 180000L;
        alarmManager.cancel(intent);
        alarmManager.set(2, time, intent);
    }

    static void cancelHeartbeat(Context context) {
        RLog.d(context, "cancelHeartbeat", context.getPackageName());
        Intent heartbeatIntent = new Intent(context, HeartbeatReceiver.class);
        heartbeatIntent.setPackage(context.getPackageName());
        PendingIntent intent = PendingIntent.getBroadcast(context, 0, heartbeatIntent, 0);
        AlarmManager alarmManager = (AlarmManager)context.getSystemService("alarm");
        alarmManager.cancel(intent);
    }
}
```

看一下HeartbeatReceiver,其实是在每次执行完定时任务后，再启动另外一个心跳定时任务，其代码如下：
```
public class HeartbeatReceiver extends BroadcastReceiver {
    public HeartbeatReceiver() {
    }

    public void onReceive(Context context, Intent intent) {
        if(NativeClient.nativeObj != null) {
            //开启下一次定时任务
            WakeLockUtils.startNextHeartbeat(context);
            NativeClient.nativeObj.ping(new PingCallback() {
                public void onSuccess() {
                    RLog.d(this, "PING", "success");
                }

                public void onError() {
                    RLog.d(this, "PING", "fail");
                }
            });
        }
    }
}

```

### 2.3 网络异常的重连逻辑
在RongIMClient绑定服务成功之后就注册了网络变更的receiver，代码如下：
```
IntentFilter filter = new IntentFilter("android.net.conn.CONNECTIVITY_CHANGE");
            IntentFilter reconnectFilter = new IntentFilter("action_reconnect");
            RongIMClient.this.mContext.registerReceiver(RongIMClient.sS.mConnectChangeReceiver, filter);
            RongIMClient.this.mContext.registerReceiver(RongIMClient.sS.mConnectChangeReceiver, reconnectFilter);
```

下面是处理网络变更及重连的Receiver，代码如下：
```
public class ConnectChangeReceiver extends WakefulBroadcastReceiver {
    public static final String RECONNECT_ACTION = "action_reconnect";

    public ConnectChangeReceiver() {
    }

    public void onReceive(Context context, Intent intent) {
        if(intent != null && intent.getAction() != null) {
            RLog.d(this, "ConnectStatusChange", intent.toString());
            ConnectivityManager cm;
            NetworkInfo networkInfo;
            if(intent.getAction().equals("android.net.conn.CONNECTIVITY_CHANGE")) {
                cm = (ConnectivityManager)context.getSystemService("connectivity");
                networkInfo = cm.getActiveNetworkInfo();
                if(networkInfo != null) {
                    if(RongIMClient.getInstance().getCurrentConnectionStatus() != ConnectionStatus.NETWORK_UNAVAILABLE && RongIMClient.getInstance().getCurrentConnectionStatus() != ConnectionStatus.DISCONNECTED) {
                        if(RongIMClient.getInstance().getNetWorkType() != -1 && networkInfo.getType() != RongIMClient.getInstance().getNetWorkType()) {
                            startWakefulService(context, new Intent(context, ReConnectService.class));
                        }
                    } else {
                        startWakefulService(context, new Intent(context, ReConnectService.class));
                    }
                }
            } else if(intent.getAction().equals("action_reconnect")) {
                RLog.d(this, "RECONNECT_ACTION", intent.toString());
                cm = (ConnectivityManager)context.getSystemService("connectivity");
                networkInfo = cm.getActiveNetworkInfo();
                if(networkInfo != null) {
                    startWakefulService(context, new Intent(context, ReConnectService.class));
                }
            }

        }
    }
}

```

当网络发生变更时，就会启动ReConnectService进行重连操作，本质上还是重新再执行connect方法
```
public class ReConnectService extends IntentService {
    public ReConnectService() {
        super("RONG_ReConnect");
    }

    protected void onHandleIntent(final Intent intent) {
        if(intent != null) {
            if(RongIMClient.getInstance() == null) {
                ConnectChangeReceiver.completeWakefulIntent(intent);
            } else {
                RLog.d(this, "RECONNECT", intent.toString());
                RongIMClient.getInstance().reconnect(new ConnectCallback() {
                    public void onSuccess(String s) {
                        ConnectChangeReceiver.completeWakefulIntent(intent);
                    }

                    public void onError(ErrorCode e) {
                        ConnectChangeReceiver.completeWakefulIntent(intent);
                    }
                });
            }
        }

    }
}

```

### 2.4 消息接收
注册消息监听器，代码如下：
```
public void setOnReceiveMessageListener(final NativeClient.OnReceiveMessageListener listener) {
        if(nativeObj == null) {
            throw new RuntimeException("NativeClient 尚未初始化!");
        } else {
            nativeObj.SetMessageListener(new ReceiveMessageListener() {
                public void onReceived(io.rong.imlib.NativeObject.Message nativeMessage, int left) {
                    Message message = new Message(nativeMessage);
                    message.setContent(NativeClient.renderMessageContent(nativeMessage.getObjectName(), nativeMessage.getContent(), message));
                    Constructor constructor = (Constructor)NativeClient.messageHandlerMap.get(nativeMessage.getObjectName());

                    try {
                        if(constructor != null) {
                            MessageHandler e = (MessageHandler)constructor.newInstance(new Object[]{NativeClient.sContext});
                            e.decodeMessage(message, message.getContent());
                        }
                    } catch (InstantiationException var6) {
                        throw new RuntimeException(var6);
                    } catch (IllegalAccessException var7) {
                        throw new RuntimeException(var7);
                    } catch (InvocationTargetException var8) {
                        throw new RuntimeException(var8);
                    }

                    if(!(message.getContent() instanceof UnknowMessage)) {
                        Log.d("RongIMClinet", "------getReceiveMessageListener--------onReceived-------");
                        if(listener != null) {
                            listener.onReceived(message, left);
                        }

                    }
                }
            });
        }
    }
```

IMKit由于使用EventBus，用于组件件通信， 防止组件单独注册消息监听器，导致代码冗余，代码如下：
```
private static final void initConnectedData(RongIMClient client) {
        client.setOnReceiveMessageListener(new OnReceiveMessageListener() {
            public boolean onReceived(final Message message, int left) {
                MessageTag msgTag = (MessageTag)message.getContent().getClass().getAnnotation(MessageTag.class);
                if(msgTag != null) {
                    if(msgTag.flag() > 0) {
                        RongIMClientWrapper.sS.mContext.getEventBus().post(new OnReceiveMessageEvent(message, left));
                        ConversationNotifyLogic.isNotify(RongContext.getInstance(), message, new ConversationNotifyCallback() {
                            public void onComplete(boolean isNotify) {
                                if(isNotify) {
                                    boolean isRunningInBackground = ConversationNotifyLogic.isRunningInBackground(RongContext.getInstance(), message);
                                    if(!isRunningInBackground) {
                                        List list = RongContext.getInstance().getCurrentConversationList();
                                        boolean isReminder = true;
                                        Iterator msgTag = list.iterator();

                                        while(msgTag.hasNext()) {
                                            ConversationInfo conversationInfo = (ConversationInfo)msgTag.next();
                                            if(message.getTargetId().equals(conversationInfo.getTargetId()) && message.getConversationType() == conversationInfo.getConversationType()) {
                                                isReminder = false;
                                                break;
                                            }
                                        }

                                        if(isReminder) {
                                            MessageTag msgTag1 = (MessageTag)message.getContent().getClass().getAnnotation(MessageTag.class);
                                            if(msgTag1 != null && (msgTag1.flag() & 2) == 2) {
                                                MessageSoundLogic.getInstance().messageReminder();
                                            }
                                        }
                                    } else {
                                        PushNotificationManager.getInstance().onReceiveMessageFromApp(message);
                                    }
                                }

                            }
                        });
                    } else if(message != null && message.getConversationType() == ConversationType.PRIVATE) {
                        String className = message.getContent().getClass().getName();
                        if(className.equals("io.rong.voipkit.message.VoIPCallMessage") || className.equals("io.rong.voipkit.message.VoIPAcceptMessage") || className.equals("io.rong.voipkit.message.VoIPFinishMessage")) {
                            RongIMClientWrapper.sS.mContext.getEventBus().post(new OnReceiveVoIPMessageEvent(message, left));
                        }
                    }
                }

                return RongIMClientWrapper.mReceiveMessageListener != null && RongIMClientWrapper.mReceiveMessageListener.onReceived(message, left);
            }
        });
        RongIMClient.setConnectionStatusListener(new ConnectionStatusListener() {
            public void onChanged(ConnectionStatus status) {
                RLog.d(this, "ConnectStatus", status.toString());
                RongIMClientWrapper.sS.mContext.getEventBus().post(status);
                if(RongIMClientWrapper.mConnectionStatusListener != null) {
                    RongIMClientWrapper.mConnectionStatusListener.onChanged(status);
                }

            }
        });
        client.getConversationNotificationQuietHours((GetConversationNotificationQuietHoursCallback)null);
    }
```
## 三. push通道协议部分
### 3.1. Push 组件启动
在RongIMClient启动的时候，其实就已经启动了Push相关组件，在其init方法有如下方法：
```
//这里是只是部分代码，仅仅做演示用
public static void init(Context context) {
        if(context == null) {
            throw new IllegalArgumentException("Context异常");
        } else {
            if(mCurrentProcessName == null) {
                mCurrentProcessName = SystemUtils.getCurProcessName(context);
            }

            RLog.d(context, "init", mCurrentProcessName);
            if(mMainProcessName == null) {
                mMainProcessName = context.getPackageName();
            }

            RongVersionDatabase.init(context);
            //启动push相关服务
            PushContext.init(context);
          }
```
### 3.2. 连接服务->心跳->中断连接过程
* 启动定时连接定时器
```
public void sendConnectCommand(Context context, String packageName) {
        Intent intent = new Intent(this, PushReceiver.class);
        intent.setAction("io.rong.push.Connect");
        intent.setPackage(TextUtils.isEmpty(packageName)?null:this.getPackageName());
        PendingIntent pendingIntent = PendingIntent.getBroadcast(context, 0, intent, 0);
        AlarmManager alarmManager = (AlarmManager)context.getSystemService("alarm");
        alarmManager.cancel(pendingIntent);
        int random = (new Random()).nextInt(50) * 100;
        alarmManager.set(3, SystemClock.elapsedRealtime() + (long)random, pendingIntent);
        RLog.i(this, "sendConnectCommand", random + "");
    }
```
* pushReceiver 消息分发，以下是onReceiver部分方法，处理启动连接逻辑：
```
                if("io.rong.push.Connect".equals(action)) {
                    if(PushContext.getInstance() != null && PushContext.getInstance().isNewestVersion() && PushContext.getInstance().validateNetworkEnable()) {
                        AppVersion syncIntent = PushContext.getInstance().getRunningPushServiceVersion();
                        if(syncIntent == null) {
                            wifi_state = new Intent(context, PushService.class);
                            wifi_state.setAction(action);
                            startWakefulService(context, wifi_state);
                            RLog.i(this, "CONNECT", "run connect");
                        } else if(PushContext.getInstance().getCurrentVersion().getPushVersionCode() > syncIntent.getPushVersionCode()) {
                            wifi_state = new Intent(context, PushService.class);
                            wifi_state.setAction(action);
                            startWakefulService(context, wifi_state);
                            RLog.i(this, "CONNECT", "run connect");
                        }
                    }
                }
```

* pushService 处理连接，心跳等逻辑
```
protected void onHandleIntent(Intent intent) {
        if(intent != null && intent.getAction() != null) {
            if(intent.getAction().equals("io.rong.push.Connect")) {
                if(PushContext.getInstance().getRunningPushServiceVersion() == null) {
                    RLog.i(this, "CONNECT_CHECK", "getRunningPushServiceVersion null");
                    this.mPushHandler.connect(intent);
                } else if(PushContext.getInstance().getRunningPushServiceVersion().getPushVersionCode() < PushContext.getInstance().getCurrentVersion().getPushVersionCode()) {
                    RLog.i(this, "CONNECT_CHECK", PushContext.getInstance().getCurrentVersion().getPushVersionCode() + ":" + PushContext.getInstance().getRunningPushServiceVersion().getPushVersionCode());
                    this.mPushHandler.connect(intent);
                } else {
                    PushReceiver.completeWakefulIntent(intent);
                }
            } else if(intent.getAction().equals("io.rong.push.HeartBeat")) {
                this.mPushHandler.heartbeat(intent);
            } else if(intent.getAction().equals("io.rong.push.Disconnect")) {
                this.mPushHandler.disConnect(intent);
            }

        }
    }
```

* pushService中处理核心逻辑
```
private final class PushHandler {
        boolean isClientConnected;
        PushClient pushClient;

        private PushHandler() {
            this.isClientConnected = false;
        }

        public void connect(Intent intent) {
            Log.i("PushHandler", "ConnectToServer,isClientConnected = " + this.isClientConnected);
            Class var2 = PushService.PushHandler.class;
            synchronized(PushService.PushHandler.class) {
                label42: {
                    if(this.isClientConnected) {
                        return;
                    }

                    try {
                        StringBuilder e = new StringBuilder();
                        String deviceId = Utils.getDeviceId(PushService.this);
                        final ArrayList params = new ArrayList();
                        params.add(new BasicNameValuePair("deviceId", deviceId));
                        JSONObject object = (JSONObject)PushContext.getInstance().getHttpHandler().executeRequestSync(new AbstractHttpRequest(2, URI.create("http://nav.cn.rong.io/navipush.json"), params, new JsonEntityParser()) {
                            public void onComplete(JSONObject o) {
                            }

                            public void onFailure(BaseException e) {
                            }
                        });
                        String val = object.optString("server");
                        RLog.i(this, "Push_IP", val);
                        e.append(val);
                        if(TextUtils.isEmpty(e)) {
                            RLog.e(this, "PushConnect", "Address error");
                        } else {
                            String[] address = e.toString().split(":");
                            this.pushClient = new PushClient(deviceId, "1", "", new PushService.PushHandler.PushClientListener(), (PingSuccessListener)null);
                            this.pushClient.connect(address[0], Integer.parseInt(address[1]), new PushService.PushHandler.ClientConnectCallback());
                            break label42;
                        }
                    } catch (Exception var10) {
                        var10.printStackTrace();
                        this.pushClient = null;
                        this.isClientConnected = false;
                        PushContext.getInstance().startNextHeartbeat(180000L);
                        break label42;
                    }

                    return;
                }
            }

            PushReceiver.completeWakefulIntent(intent);
        }

        public void disConnect(Intent intent) {
            if(this.pushClient != null) {
                this.pushClient.disconnectByNormal();
            }

            if(intent != null) {
                PushReceiver.completeWakefulIntent(intent);
            }

        }

        public boolean isClientConnected() {
            return this.isClientConnected;
        }

        public void heartbeat(Intent intent) {
            if(PushContext.getInstance().validateNeedSyncVersion()) {
                PushContext.getInstance().syncVersion();
            }

            if(!PushContext.getInstance().isNewestVersion()) {
                PushContext.getInstance().cancelHeartbeat();
                AppVersion e = PushContext.getInstance().getNewestVersion();
                PushContext.getInstance().sendConnectCommand(PushService.this, e.getAppId());
                this.disConnect(intent);
                PushService.this.stopSelf();
                PushReceiver.completeWakefulIntent(intent);
            } else if(this.pushClient != null && this.isClientConnected) {
                try {
                    this.pushClient.ping();
                } catch (IOException var3) {
                    RLog.e(this, "Heartbeat", var3.getMessage(), var3);
                    if(this.pushClient != null) {
                        this.pushClient.disconnectByNormal();
                    }

                    this.pushClient = null;
                    this.isClientConnected = false;
                    this.connect(intent);
                    var3.printStackTrace();
                }

                PushContext.getInstance().startNextHeartbeat(180000L);
                PushReceiver.completeWakefulIntent(intent);
            } else {
                this.connect(intent);
            }
        }

        private Bundle parsePushMsgFromJson(String jstr) throws JSONException {
            if(TextUtils.isEmpty(jstr)) {
                Log.e("PushService", "parsePushMsgFromJson jstr is null");
            }

            Bundle bundle = new Bundle();
            JSONObject json = new JSONObject(jstr);
            Log.i("PushService", "parsePushMsgFromJson");
            bundle.putString("objectName", json.optString("objectName"));
            bundle.putString("packageName", json.optString("packageName"));
            bundle.putString("appId", json.optString("appId"));
            bundle.putString("fromUserId", json.optString("fromUserId"));
            bundle.putString("fromUserName", json.optString("fromUserName"));
            bundle.putString("title", json.optString("title"));
            bundle.putString("content", json.optString("content"));
            bundle.putString("channelType", json.optString("channelType"));
            bundle.putString("channelId", json.optString("channelId"));
            bundle.putString("channelName", json.optString("channelName"));
            return bundle;
        }

        private class PushClientListener implements ClientListener {
            private PushClientListener() {
            }

            public void messageArrived(PublishMessage msg) {
                Bundle bundle = null;
                Intent intent = new Intent();
                if(msg != null && msg.getDataAsString() != null) {
                    RLog.i(PushService.this, "Received", msg.getDataAsString());

                    try {
                        bundle = PushHandler.this.parsePushMsgFromJson(msg.getDataAsString());
                    } catch (JSONException var9) {
                        System.err.println("Error json string!");
                        var9.printStackTrace();
                        return;
                    }

                    String appId = bundle.getString("appId");
                    Version version = (Version)RongVersionDatabase.getVersionDao().queryBuilder().where(Properties.AppKey.eq(appId), new WhereCondition[0]).limit(1).unique();
                    if(version != null) {
                        Log.i("PushService", "the package name with the appId is " + bundle.getString("packageName"));

                        try {
                            PushService.this.getPackageManager().getPackageInfo(version.getAppId(), 0);
                        } catch (NameNotFoundException var8) {
                            var8.printStackTrace();
                            RongVersionDatabase.getVersionDao().delete(version);
                            return;
                        }

                        intent.setAction("io.rong.push.message");
                        intent.setPackage(bundle.getString("packageName"));
                        intent.putExtras(bundle);
                        PushService.this.sendBroadcast(intent);
                    }
                } else {
                    RLog.i(PushService.this, "Received", "the message received from server is null!!!");
                }
            }
        }

        private class ClientConnectCallback implements ConnectStatusCallback {
            private ClientConnectCallback() {
            }

            public void onConnected(ConnAckMessage msg) throws IOException {
                Log.i("PushService", "The client connect status is:" + msg.getType());
                if(msg.getType() == Type.CONNACK) {
                    if(msg.getStatus() == ConnectionStatus.ACCEPTED) {
                        PushHandler.this.isClientConnected = true;
                        RLog.i(PushService.this, "Connect", "Client connect successfully.So start the heart beat.");
                    } else {
                        RLog.e(PushService.this, "Connect to server failure!! Error code :", msg.getStatus().toString());
                    }

                    PushContext.getInstance().startNextHeartbeat(180000L);
                }

            }

            public void onDisconnected(IOException e) {
                RLog.e(PushService.this, "ConnectStatus", e.toString());
                if(!PushContext.getInstance().hasNetworkInfo()) {
                    PushContext.getInstance().cancelHeartbeat();
                }

                PushHandler.this.pushClient = null;
                PushHandler.this.isClientConnected = false;
            }
        }
    }
```

* pushClient负责与服务器建立socket连接

```
class PushClient {
    private MessageInputStream in;
    private Socket socket;
    private MessageOutputStream out;
    public OutputStream os;
    private PushClient.PushReader reader;
    private Semaphore connectionAckLock;
    private PushClient.ClientListener listener;
    private ConnectMessage connectMsg;
    private PushClient.ConnectStatusCallback connectCallback;
    private PushClient.PingSuccessListener mPingSuccessListener;

    public PushClient(String clientId, String appkey, String token, PushClient.ClientListener listener, PushClient.PingSuccessListener mPingSuccessListener) {
        this.listener = listener;
        this.mPingSuccessListener = mPingSuccessListener;
        this.connectMsg = new ConnectMessage(clientId, true, 300);
        this.connectMsg.setCredentials(appkey, token);
        this.connectMsg.setWill("clientInfo", String.format("%s-%s-%s", new Object[]{"testClient", "1.0", "1.0"}));
    }

    public void connect(String host, int port, PushClient.ConnectStatusCallback callback) throws IOException, InterruptedException {
        SocketChannel socketChannel = SocketChannel.open();
        this.socket = new Socket();
        RLog.i(this, "connect", "the socket is established");
        InetSocketAddress address = new InetSocketAddress(host, port);
        this.socket.connect(address, 4000);
        InputStream is = this.socket.getInputStream();
        this.in = new MessageInputStream(is);
        this.os = this.socket.getOutputStream();
        this.out = new MessageOutputStream(this.os);
        this.reader = new PushClient.PushReader((PushClient.SyntheticClass_1)null);
        this.reader.start();
        this.connectCallback = callback;
        this.connectionAckLock = new Semaphore(0);
        this.out.writeMessage(this.connectMsg);
        this.connectionAckLock.acquire();
    }

    public void ping() throws IOException {
        if(this.socket != null && this.socket.isConnected() && this.out != null) {
            RLog.i(this, "Ping", this.socket.getInetAddress().toString());
            this.out.writeMessage(new PingReqMessage());
            if(this.mPingSuccessListener != null) {
                this.mPingSuccessListener.onSuccess();
            }
        }

    }

    public void disconnect() throws IOException {
        if(this.socket != null) {
            RLog.i(this, "connect", "the socket is closed");
            this.socket.close();
        }

    }

    public void disconnectByNormal() {
        DisconnectMessage msg = new DisconnectMessage(DisconnectionStatus.CLOSURE);
        if(this.out != null && this.socket != null) {
            try {
                this.out.writeMessage(msg);
            } catch (IOException var11) {
                var11.printStackTrace();
            } finally {
                try {
                    this.socket.close();
                } catch (IOException var10) {
                    var10.printStackTrace();
                }

            }

        }
    }

    private void handleMessage(Message msg) throws IOException {
        if(msg != null) {
            RLog.i(this, "Handler", msg.getType() + "");
            switch(PushClient.SyntheticClass_1.$SwitchMap$io$rong$push$PushProtocalStack$Message$Type[msg.getType().ordinal()]) {
            case 1:
                this.connectionAckLock.release();
                if(this.connectCallback != null) {
                    ConnAckMessage publishMsg1 = (ConnAckMessage)msg;
                    this.connectCallback.onConnected(publishMsg1);
                }
                break;
            case 2:
                if(this.mPingSuccessListener != null) {
                    this.mPingSuccessListener.onSuccess();
                }
                break;
            case 3:
                PublishMessage publishMsg = (PublishMessage)msg;
                if(this.listener != null) {
                    Log.i("PushClient", "call Publish msg listener");
                    this.listener.messageArrived(publishMsg);
                }
            }

        }
    }

    public interface PingSuccessListener {
        void onSuccess();

        void onFailure();
    }

    public interface ConnectStatusCallback {
        void onConnected(ConnAckMessage var1) throws IOException;

        void onDisconnected(IOException var1);
    }

    public interface ClientListener {
        void messageArrived(PublishMessage var1);
    }

    private class PushReader extends Thread {
        private PushReader() {
        }

        public void run() {
            Message msg = null;

            try {
                while(true) {
                    try {
                        Thread.sleep(100L);
                    } catch (Exception var3) {
                        ;
                    }

                    if(PushClient.this.in != null) {
                        msg = PushClient.this.in.readMessage();
                    }

                    if(msg != null) {
                        PushClient.this.handleMessage(msg);
                    }
                }
            } catch (IOException var4) {
                if(PushClient.this.connectCallback != null) {
                    PushClient.this.connectCallback.onDisconnected(var4);
                }

            }
        }
    }
}

```

总结：
融云消息服务集成在lib中，利用native方法调用
与服务端保持通信连接，利用socket建立连接
pushService负责与push保持连接，包括ping，connect，receiveMessage
pushReceiver 接收系统及应用广播
pushContext 中转pushReceiver消息给pushService处理
