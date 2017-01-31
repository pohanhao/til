# Decorator pattern

```java
public interface IEmailService
{
    void send(Email email);
    Collection<EmailInfo> listEmails(int indexBegin, int indexEnd);
    Email downloadEmail(EmailInfo emailInfo);
}
```

decorated with retry
```java
public class EmailServiceRetryDecorator implements IEmailService
{
    private final IEmailService emailService;
    public EmailServiceRetryDecorator(IEmailService emailService) {
        this.emailService = emailService;
    }
    @Override
    public void send(Email email) {
        executeWithRetries(() -> emailService.send(email));
    }
    @Override
    public Collection<EmailInfo> listEmails(int indexBegin, int indexEnd) {
        final List<EmailInfo> emailInfos = new ArrayList<>();
        executeWithRetries(() -> emailInfos.addAll(emailService.listEmails(indexBegin, indexEnd)));
        return emailInfos;
    }
    @Override
    public Email downloadEmail(EmailInfo emailInfo) {
        final Email[] email = new Email[1];
        executeWithRetries(() -> email[0] = emailService.downloadEmail(emailInfo));
        return email[0];
    }
    private void executeWithRetries(Runnable runnable) {
        for(int i=0; i<3; ++i) {
            try {
                runnable.run();
            } catch (EmailServiceTransientError e) {
                continue;
            }
            break;
        }
    }
}
```

decorators queue

```java
IEmailService emailServiceWithRetriesAndCaching = new EmailServiceCacheDecorator(
  new EmailServiceRetryDecorator(new EmailService()));
```

```java
IEmailService emailService = new EmailServiceLoggingDecorator(new EmailServiceRetryDecorator(
        new EmailServiceLoggingDecorator(new EmailService())));
```

**Reference**

1. https://dzone.com/articles/is-inheritance-dead
