# IKE v1

## 简介

根据RFC 2409描述，IKEv1是一个协议，此协议使用了Oakley的一部分，以及SKEME的一部分，并结合了ISAKMP，它使用ISAKMP来获取用于IETF IPSec DOI的材料，这些材料用于生成密钥和其他的SA（例如AH，ESP）。***简单的来说，IKEv1是一个混合协议。***

***ISAKMP***提供了一个认证和密钥交换的框架，但没有具体的定义它。ISAKMP被设计用来独立的密钥交换；也就是说此框架被设计成支持多种不同的密钥交换。

***Oakley***描述了一系列的密钥交换，被称为“模式”，详细描述了每一个提供的服务（例如，密钥的完全的后继保密(perfect forward secrecy)，身份保护和认证）。

***SKEME***描述了一种通用的密钥交换技术，此密钥交换技术提供了匿名性，可否认性和密钥的快速更新。

## 阶段

### Phase1

Phase1中，两个ISAKMP节点建立一个安全的、已认证的用于通信的通道。这被称为“ISAKMP SA”。完成此阶段的交换，有两个模式：“Main Mode”和“Aggressive Mode”。上述的两个模式只能用于Phase1。


### Phase2

Phase2中协商了重要服务的SA，例如IPSec，或者其他的需要密钥材料或协商参数的服务。“Quick Mode”完成Phase2的交换，此模式只能用于Phase2。

“New Group Mode”既不是phase1，也不是phase2。它紧跟phase1，用来建立一个新组，此新组用于将来的协商。它只能在phase1之后使用。


## 标记

>
1. HDR---ISAKMP的报文头，交换的类型是模式。HDR* 表明它的payload是加密的。
2. SA---是一个SA协商payload，此payload中有一个或多个提议。一个发起者需要提供多个协商提议；一个响应者必须使用一个提议回复。
3. \<P>_b --- 是\<P> payload 的body部分，不含ISAKMP的通用vpayload。
4. SAi_b --- 是SA payload的完整的body部分（数据部分？），除去IDAKMP的通用报文头。例如发起方的DOI，情况，所有的提议，以及所有的transform。
5. CKI-I 和 CKI-R 分别是ISAKMP报文头中的发起方和响应方的cookie。
6. g^xi 和 g^xr分别是发起方和响应方的DH的公开值。
7. g^xy --- 是DH的共享秘密（shared secret）。
8. KE --- 密钥交换的payload，此payload包含了DH交换中的公开交换信息。对于KE payload 数据，无特定的编码，例如TLV。
9. Ni 和 Nr --- 分别是发起方和响应方的当前nonce的payload。
10. IDx --- 当前“x”的身份识别的payload。当"x"为"ii"或“ir”时，分别表示phase1中的ISAKMP发起方和响应方；当"x"为"ui"或"ur"，分别表示phase2中的用户发起方(user initiator)和响应方。因特网中 DOI中ID payload的格式在[Pip97]中定义。
11. SIG --- 是payload的签名。签名的数据是特定于某种交换的。
12. CERT --- 是证书payload。
13. HASH --- 是hash的payload。
14. prf(key, msg) --- 是key的伪随机数，通常是key的hash函数，用于产生表现有伪随机性的确定性输出。prf用于密钥的计算和验证。
15. SKEYID --- 是从秘密材料(secret material)推导出来的字符串，只有交换中活跃方才知道。
16. SKEID_e --- 是密钥材料（keying material），ISAKMP使用此材料保护消息的机密性。
17. SKEID_a --- 是密钥材料，ISAKMP使用此材料认证它的消息。
18. SKEID_d --- 是密钥材料，用于推导出非ISAKMP SA的密钥。
19. \<x>y --- 表示"x"由密钥y加密生成。
20. --> --- 表示发起方到响应方的通信。
21. <-- --- 表示响应方到发起方的通信。
22. | --- 表示信息的串联，例如 x|y 表示x和y是串联的。
23. [x] --- 表示x是可选的。


## 交互

有两种基本的方法，用来建立经过认证的密钥交换：Main Mode和Aggressive Mode。它们都通过短暂的DH交换来产生经过验证的密钥材料。


### phase1

#### 使用签名认证的phase1

##### Main mode

交互过程如下所示：

```
   Initiator                          Responder
  -----------                        -----------
   HDR, SA                      -->
                                <--    HDR, SA
   HDR, KE, Ni                  -->
                                <--    HDR, KE, Nr
   HDR\*, IDii, [ CERT, ] SIG_I -->
                                <--    HDR*, IDir, [ CERT, ] SIG_R
```

***说明：***

1. 首先发起方发送一个ISAKMP的SA，包含有很多的提议。例如：支持的加密认证算法族。
2. 响应方回复一个ISAKMP SA，包含一个提议。例如：某个加密认证算法。
3. 以上两个构成一个交互，此交互协商策略，完成了ISAKMP的SA协商。
4. 发送方发送DH协商的公开值和辅助数据。
5. 响应方回复DH协商的公开值和辅助数据。
6. 4、5构成一个交互，完成了DH的交互。
7. 发起方发送加密的ISAKMP报文头、发起方的身份信息、证书（可选）、发起方的签名。
8. 响应方发送加密的ISAKMP报文头、响应方的身份信息、证书（可选）、响应方的签名。
9. 7、8验证4、5完成的DH交换。phase1结束。

##### Aggressive mode

交互过程如下：

```
    Initiator                          Responder
   -----------                        -----------
HDR, SA, KE, Ni, IDii   -->

                        <--         HDR, SA, KE, Nr, IDir,
                                       [ CERT, ] SIG_R
                              
HDR, [ CERT, ] SIG_I    -->

```

***说明：***

1. 首先发起方发送ISAKMP的SA、DH交换的发起方公开值、发起方的nonce、发起方的身份信息。
2. 响应方回复一个ISAKMP的SA、DH交换的响应方公开值、响应方的nonce、响应方的身份信息、响应方证书（可选）、响应方的签名。
3. 1、2交互协商了策略、DH交换的公开值以及必要的辅助数据、以及响应方的身份认证。
4. 发起方发送发起方的证书（可选），发起方的签名。
5. 4中对发起方进行认证、并提供交换的证据，即签名。


#### 使用公钥加密认证的phase1

##### Main mode

交互过程如下：

```
      Initiator                        Responder
     -----------                      -----------
      HDR, SA                   -->
                                <--    HDR, SA
      HDR, KE, [ HASH(1), ]
          <IDii_b>PubKey_r,
          <Ni_b>PubKey_r        -->
                                         HDR, KE, <IDir_b>PubKey_i,
                                <--            <Nr_b>PubKey_i
      HDR*, HASH_I              -->
                                <--    HDR*, HASH_R
```

***说明：***


##### Aggressive mode

交互过程如下：

```
      Initiator                        Responder
     -----------                      -----------
      HDR, SA, [ HASH(1),] KE,
        <IDii_b>Pubkey_r,
         <Ni_b>Pubkey_r         -->
           
                                     HDR, SA, KE,  <IDir_b>PubKey_i,
                                <--         <Nr_b>PubKey_i, HASH_R
                                
      HDR, HASH_I               -->
```

***说明：***


#### 使用改进的公钥加密认证的phase1

##### Main mode

交互过程如下：

```
     Initiator                        Responder
    -----------                      -----------
     HDR, SA                   -->
                               <--    HDR, SA
                        
     HDR, [ HASH(1), ]
     <Ni_b>Pubkey_r,
     <KE_b>Ke_i,
     <IDii_b>Ke_i,
     [<<Cert-I_b>Ke_i]         -->
     
                                      HDR, <Nr_b>PubKey_i,
                                      <KE_b>Ke_r,
                               <--    <IDir_b>Ke_r,
                                  
     HDR*, HASH_I              -->
        
                               <--    HDR*, HASH_R
                        
```

***说明：***


##### Aggressive mode

交互过程如下：

```
      Initiator                        Responder
     -----------                      -----------
      HDR, SA, [ HASH(1),]
       <Ni_b>Pubkey_r,
       <KE_b>Ke_i, <IDii_b>Ke_i
       [, <Cert-I_b>Ke_i ]      -->
       
                                      HDR, SA, <Nr_b>PubKey_i,
                                      <KE_b>Ke_r, <IDir_b>Ke_r,
                                <--         HASH_R
                                  
      HDR, HASH_I               -->
```

***说明：***


#### 使用共享密钥认证的phase1

##### Main mode

交互过程如下：

```
      Initiator                        Responder
     ----------                       -----------
      HDR, SA             -->
                          <--    HDR, SA
      HDR, KE, Ni         -->
                          <--    HDR, KE, Nr
      HDR*, IDii, HASH_I  -->
                          <--    HDR*, IDir, HASH_R

```

***说明：***


##### Aggressive mode

交互过程如下：

```
      Initiator                        Responder
     -----------                      -----------
      HDR, SA, KE, Ni, IDii -->
                            <--    HDR, SA, KE, Nr, IDir, HASH_R
      HDR, HASH_I           -->
```

***说明：***

******************

### phase2


******************

## 报文


## 参考

1. [RFC 2409](https://www.ietf.org/rfc/rfc2409.txt)