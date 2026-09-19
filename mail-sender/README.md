# 📧 Kanidm Mail Sender

```sh
kanidm service-account create mail-sender "Mail Sender" idm_admins
```

```sh
kanidm group add-members idm_message_senders mail-sender
```

```sh
kanidm service-account api-token generate mail-sender "mail sender token" --readwrite
```

Testing:

```sh
kanidm system message-queue send-test-message tester
```

```sh
kanidm system message-queue list
```