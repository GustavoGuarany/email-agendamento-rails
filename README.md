
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

```ruby
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

### 4. Configuração sidekiq
Adicione um arquivo de inicialização em `config/initializers/sidekiq.rb`:

```ruby
require 'sidekiq'

Sidekiq.configure_server do |config|
  config.redis = { url: 'redis://localhost:6379/0' }
end

Sidekiq.configure_client do |config|
  config.redis = { url: 'redis://localhost:6379/0' }
end
```

### 5. Adicione um trabalho 
`app/sidekiq` para processar trabalhos de forma assíncrona:

```ruby
rails generate sidekiq:job exemplo
```
Ele gera o arquivo de `Worker`, bem como o teste ou especificação, dependendo da estrutura de teste que você configurou.

```ruby
class ExemploJob
  include Sidekiq::Job

  def perform(name, count)
    # faça algo
    puts "Working hard on #{name} for #{count} times"
  end
end
```

O método `perform` é o método onde você coloca a lógica do seu trabalho.

Colocar um Job na Fila:

Você pode enfileirar trabalhos para serem processados:

```ruby
HardWorker.perform_async('Bob', 5)
```
Ou, se você quiser que o trabalho seja executado em algum momento no futuro:

```ruby
HardWorker.perform_in(5.minutes, 'Bob', 5)
```

### 6. Configurar Rufus-Scheduler
Crie um arquivo em `config/initializers/scheduler.rb`:

```ruby
require 'rufus-scheduler'

s = Rufus::Scheduler.singleton

s.every '1m' do
  # Aqui você pode buscar tarefas e agendar e-mails
  # Por exemplo:
  # tasks = Task.where("created_at <= ?", Time.now - 5.minutes)
  # tasks.each do |task|
  #   TaskWorker.perform_async(task.email, task)
  # end
end
```
### 7. Configurar Redis
Certifique-se de que você tenha o Redis instalado e rodando. Se você não tiver, pode instalá-lo com:

```console
sudo apt update
sudo apt install redis-server
```
Inicie o servidor Redis:

```console
redis-server
```

### 8. Rodar o Sidekiq:
 
Abra um terminal e execute:
```console
bundle exec sidekiq
```






