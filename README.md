
# email-agendamento-rails

# Envio e agendamento de email com action mailer, sidekiq e rufus-scheduler

### 1. Gems

```console
gem 'sidekiq'
gem 'rufus-scheduler'
```
### 2. Configurar o Action Mailer

Primeiro, você precisa configurar o `Action Mailer`. Crie um novo arquivo em `app/mailers/application_mailer.rb` se ainda não existir:

```ruby
class ApplicationMailer < ActionMailer::Base
  default from: 'from@example.com'
  layout 'mailer'
end
```
### 3. Criar um Mailer Específico

```console
rails generate mailer ExemploMailer
```
Isso vai gerar um arquivo em `app/mailers/task_mailer.rb` Nele, adicione um `método` para enviar notificações:

```ruby
class ExemploMailer < ApplicationMailer
  def task_notification(email, task)
    @task = task
    mail(to: email, subject: 'Nova Tarefa Criada')
  end
end
```




