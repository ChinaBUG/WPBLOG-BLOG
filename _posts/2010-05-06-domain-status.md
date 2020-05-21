---
ID: 508
post_title: 域名状态说明
author: ChinaBUG
post_excerpt: |
  RRP域名状态
  ACTIVE
  REGISTRY-LOCK(REGISTRYLOCK)
  REGISTRY-HOLD(REGISTRYHOLD)
  REGISTRAR-LOCK(REGISTRARLOCK)
  REGISTRAR-HOLD(REGISTRARHOLD)
  REDEMPTIONPERIOD(REDEMPTIONPERIOD)
  PENDING RESTORE(PENDINGRESTORE)
  PENDING DELETE(PENDINGDELETE)
  
  EPP域名状态
  Status: OK
  Status: INACTIVE
  Status: CLIENT TRANSFER PROHIBITED(CLIENTTRANSFERPROHIBITED)
  Status: CLIENT DELETE PROHIBITED(CLIENTDELETEPROHIBITED)
  Status: CLIENT UPDATE PROHIBITED(CLIENTUPDATEPROHIBITED)
  Status: CLIENT RENEW PROHIBITED(CLIENTRENEWPROHIBITED)
  Status: PENDING TRANSFER(PENDINGTRANSFER)
  Status: PENDING UPDATE(PENDINGUPDATE)
  Status: PENDING RENEW(PENDINGRENEW)
  Status: PENDING DELETE(PENDINGDELETE)
  Status: SERVER HOLD(SERVERHOLD)
  Status: CLIENT HOLD(CLIENTHOLD)
  Status: SERVER DELETE PROHIBITED(SERVERDELETEPROHIBITED)
  Status: SERVER UPDATE PROHIBITED(SERVERUPDATEPROHIBITED)
  Status: SERVER TRANSFER PROHIBITED(SERVERTRANSFERPROHIBITED)
  Status: SERVER RENEW PROHIBITED(SERVERRENEWPROHIBITED)
  Status: SERVER LOCK(SERVERLOCK)
  Status: CLIENT DELETE PROHIBITED(CLIENTDELETEPROHIBITED)
  Status: CLIENT UPDATE PROHIBITED(CLIENTUPDATEPROHIBITED)
  Status: CLIENT TRANSFER PROHIBITED(CLIENTTRANSFERPROHIBITED)
  Status: CLIENT LOCK(CLIENTLOCK)
  Status: REDEMPTION PERIOD(REDEMPTIONPERIOD)
  Status: PENDING RESTORE(PENDINGRESTORE)
layout: post
permalink: >
  http://blog.ipodmp.com/2010/05/domain-status.html
published: true
post_date: 2010-05-06 10:24:17
---
　　目前域名状态有23种，域名状态分为<span style="text-decoration: underline;"><strong>RRP</strong></span>域名状态和<span style="text-decoration: underline;"><strong>EPP</strong></span>域名状态。

　　RRP域名状态是指“注册局-注册商协议域名状态代码”所规定的状态[Registry Registrar Protocol （RRP） Domain Status Codes]：RRP由Verisign开发定义，用于.com/.net域名状态(例如：wansai.com或wansai.net)，部分国家域名也采用该方法。RRP域名状态有八种。

　　EPP域名状态是指“可扩展规定协议域名状态代码”所规定的状态[Extensible Provisioning Protocol （EPP） Domain Status Codes]：ORG/.BIZ /.INFO(例如：wansai.org/wansai.biz/wansai.info) 以及 .Name 注册局使用EPP域名状态定义。相比RRP状态，EPP状态更容易理解、更加详细，如：CLIENT DELETE PROHIBITED(CLIENTDELETEPROHIBITED)表示客户端不可以删除域名，EPP状态中的SERVER和CLIENT指RRP协议中的注册局和注册商。

<span style="text-decoration: underline;"><strong>RRP域名状态</strong></span>

<span style="text-decoration: underline;">ACTIVE</span>

　　活动状态。由Registry设置；该域名可以由Registrar更改；可以续费；至少被指派一个DNS。

<span style="text-decoration: underline;">REGISTRY-LOCK(<span style="text-decoration: underline;">REGISTRYLOCK</span>)</span>

　　注册局锁定。由注册局设置；该域名不可以由注册商更改、删除；必须由注册局解除此状态才可以由注册商更改域名信息；域名可以续费；如果域名被指派至少一个DNS则可以包含在(域名根服务器)的区域中(可以正常使用)。

<span style="text-decoration: underline;">REGISTRY-HOLD(<span style="text-decoration: underline;">REGISTRYHOLD</span>)</span>

　　注册局保留。由注册局设置；该域名不可以由注册商更改、删除；必须由注册局解除此状态才可以由注册商更改域名信息；域名可以续费；该域名不包括在(域名根服务器)的区域中(不能正常使用)。

<span style="text-decoration: underline;">REGISTRAR-LOCK(<span style="text-decoration: underline;">REGISTRARLOCK</span>)</span>

　　注册商锁定。由该域名的原始注册商设置；该域名不可以被更改或删除；必须由注册商解除此状态才可以更改域名信息；该域名可以续费。该域名包含在(域名根服务器)的区域中(可以正常使用)。

<span style="text-decoration: underline;">REGISTRAR-HOLD(<span style="text-decoration: underline;">REGISTRARHOLD</span>)</span>

　　注册商保留。由该域名的原始注册商设置；该域名不可以被更改或删除；必须由注册商解除此状态才可以更改域名信息；该域名可以续费。该域名不包括在(域名根服务器)的区域中(不能正常使用)。

<span style="text-decoration: underline;">REDEMPTIONPERIOD</span>

　　宽限期。当注册商向注册局提出删除域名请求后，由注册局将域名设置称此状态，不过，条件是该域名已经注册了5天以上（如果该域名注册时间不足5天，则立即删除）；该域名不包括在(域名根服务器)的区域中(不能正常使用)；该域名不可以被更改或清除，只可以被恢复；任何其他注册商提出对此域名的更改或其他请求都将被拒绝；该状态最多保持30天。

<span style="text-decoration: underline;">PENDINGRESTORE</span>

　　恢复未决。当注册商提出将处于REDEMPTIONPERIOD的域名恢复请求后，由注册局设置；该域名包含在(域名根服务器)的区域中(可以正常使用)；注册商提出的更改或任何其他请求都将被拒绝；在7天之内，有注册商向注册局提供必需的恢复文件，如果注册商在7天之内提供了这些文件，该域名将被置为ACTIVE状态，否则，该域名将重新返回到REDEMPTIONPERIOD状态。

<span style="text-decoration: underline;">PENDINGDELETE</span>

　　删除未决。如果一个域名在被设置成REDEMPTIONPERIOD状态期间内，注册商没有提出恢复请求，那么，域名将被置于PENDINGDELETE状态，注册商对此域名的任何请求都将被拒绝；5天之后清除。

<span style="text-decoration: underline;"><strong>EPP域名状态</strong></span>

<span style="text-decoration: underline;">Status: OK</span>

　　默认的正常状态：该状态由注册商设置，域名可以更新（域名信息修改）、续费、删除、转移，指派至少一台域名服务器则可以正常使用。

<span style="text-decoration: underline;">Status: INACTIVE</span>

　　非激活状态：属于正常状态，有些小问题没有设置好，比如没有指派至少一台域名服务器等。域名可以更新（域名信息修改）、续费、删除、转移.

<span style="text-decoration: underline;">Status: CLIENT TRANSFER PROHIBITED(CLIENTTRANSFERPROHIBITED)</span>

　　客户端禁止转移：该状态由注册商设置，域名可以更新（域名信息修改）、续费、删除，指派至少一台域名服务器则可以正常使用。

<span style="text-decoration: underline;">Status: CLIENT DELETE PROHIBITED(CLIENTDELETEPROHIBITED)</span>

　　客户端禁止删除：该状态由注册商设置，域名可以更新（域名信息修改）、续费、转移，指派至少一台域名服务器则可以正常使用。

<span style="text-decoration: underline;">Status: CLIENT UPDATE PROHIBITED(CLIENTUPDATEPROHIBITED)</span>

　　客户端禁止更新：该状态由注册商设置，域名可以转移、续费、删除，指派至少一台域名服务器则可以正常使用。

<span style="text-decoration: underline;">Status: CLIENT RENEW PROHIBITED(CLIENTRENEWPROHIBITED)</span>

　　客户端禁止续费：该状态由注册商设置，域名可以更新（域名信息修改）、转移、删除，指派至少一台域名服务器则可以正常使用。

<span style="text-decoration: underline;">Status: PENDING TRANSFER(PENDINGTRANSFER)</span>

　　即将转移：该状态由注册局设置，转移请求已经被接纳，域名不能更新（域名信息修改）、不能续费、不能删除，指派至少一台域名服务器则可以正常使用。

<span style="text-decoration: underline;">Status: PENDING UPDATE(PENDINGUPDATE)</span>

　　即将更新：该状态由注册局设置，更新（域名信息修改）请求已经被接纳，现在的更新很快，本状态基本不会出现，指派至少一台域名服务器则可以正常使用。

<span style="text-decoration: underline;">Status: PENDING RENEW(PENDINGRENEW)</span>

　　即将续费：该状态由注册局设置，续费请求已经被接纳, 现在的续费很快，本状态基本不会出现，指派至少一台域名服务器则可以正常使用。

<span style="text-decoration: underline;">Status: PENDING DELETE(PENDINGDELETE)</span>

　　即将删除：该状态由注册局设置，注册商提出的删除请求已被接纳，此状态.cn的一般保持15天，如果没有接到注册商的续费请求，第16天将会被删除；.com和.net的一般保持5天，来自注册商的一切请求将会被拒绝，第6天删除。

<span style="text-decoration: underline;">Status: SERVER HOLD(SERVERHOLD)</span>

　　注册局留置：该状态由注册局设置，域名的一切操作将被拒绝，指派至少一台域名服务器则可以正常使用。

<span style="text-decoration: underline;">Status: CLIENT HOLD(CLIENTHOLD)</span>

　　注册商留置：该状态由注册商设置，域名可以续费，但不能正常使用。

<span style="text-decoration: underline;">Status: SERVER DELETE PROHIBITED(SERVERDELETEPROHIBITED)</span>

　　注册局禁止删除：该状态由注册局设置，域名可以更新（域名信息修改）、转移、续费，指派至少一台域名服务器则可以正常使用。

<span style="text-decoration: underline;">Status: SERVER UPDATE PROHIBITED(SERVERUPDATEPROHIBITED)</span>

　　注册局禁止更新：该状态由注册商设置，域名可以转移、续费、删除，指派至少一台域名服务器则可以正常使用。

<span style="text-decoration: underline;">Status: SERVER TRANSFER PROHIBITED(SERVERTRANSFERPROHIBITED)</span>

　　注册局禁止转移：该状态由注册局设置，域名可以更新（域名信息修改）、续费、删除，指派至少一台域名服务器则可以正常使用。

<span style="text-decoration: underline;">Status: SERVER RENEW PROHIBITED(SERVERRENEWPROHIBITED)</span>

　　注册局禁止续费：该状态由注册局设置，域名可以更新（域名信息修改）、转移、删除，指派至少一台域名服务器则可以正常使用。

<span style="text-decoration: underline;">Status: SERVER LOCK(SERVERLOCK)</span>

　　注册局锁定：该状态由注册局设置，域名可以更新（域名信息修改）、续费、删除，指派至少一台域名服务器则可以正常使用。

<span style="text-decoration: underline;">Status: CLIENT DELETE PROHIBITED(CLIENTDELETEPROHIBITED)</span>

　　注册商禁止删除：该状态由注册商设置，域名可以更新（域名信息修改）、续费、转移，指派至少一台域名服务器则可以正常使用。

<span style="text-decoration: underline;">Status: CLIENT UPDATE PROHIBITED(CLIENTUPDATEPROHIBITED)</span>

　　注册商禁止更新：该状态由注册商设置，域名可以转移、续费、删除，指派至少一台域名服务器则可以正常使用。

<span style="text-decoration: underline;">Status: CLIENT TRANSFER PROHIBITED(CLIENTTRANSFERPROHIBITED)</span>

　　注册商禁止转移：该状态由注册商设置，域名可以更新（域名信息修改）、续费、删除，指派至少一台域名服务器则可以正常使用。

<span style="text-decoration: underline;">Status: CLIENT LOCK(CLIENTLOCK)</span>

　　注册商锁定：该状态由注册商设置，域名的操作至少有一项不能使用：更新（域名信息修改）、续费、删除、转移，指派至少一台域名服务器则可以正常使用。

<span style="text-decoration: underline;">Status: REDEMPTION PERIOD(REDEMPTIONPERIOD)</span>

　　赎回期：该状态由注册局设置，域名不可以被更改或删除，只可以被恢复；任何其他注册商提出对此域名的更改或其他请求都将被拒绝；该状态最多保持30天，域名不能正常使用。

<span style="text-decoration: underline;">Status: PENDING RESTORE(PENDINGRESTORE)</span>

　　即将恢复：原注册商对处于赎回期的域名向注册局提出RESTORE请求后，设置该状态，域名不可以更新（域名信息修改）、不能续费、不能删除，该状态可以保持7天。若原注册商没有在特定时间内提交恢复请求文件，域名即退回赎回期状态。