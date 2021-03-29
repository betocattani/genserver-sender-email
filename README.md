# Sender

An simple Elixir application using GenServer and implements the basic GenServer callbacks to simulate a Send Server email.

## Features:
- Send a email to specific user email
- Send a email to a list of users emails
- Functionality of Retry  to send the emails with status `failed`
- Default and custom `max_retries` to the server 

## Installation
- Clone the repository
- Access the project path `cd genserver-sender-email`
- Install dependencies: `mix deps.get`

## Send emails
```elixir
# Start the GenServer
iex> {:ok, pid} = GenServer.start(SendServer, max_retries: 2)

# callback handle_cast to send the email
iex> GenServer.cast(pid, {:send, "joe@email.com"})
=> :ok
=> Email to joe@email.com sent

# callback handle_cast to send the email
iex> GenServer.cast(pid, {:send, "cattani@mail.com"})
=> :ok
=> Retrying email cattani@mail.com...
=> Retrying email cattani@mail.com...

# callback handle_cal to get the current state of gen server
iex> GenServer.call(pid, :get_state)
%{
  emails: [
    %{email: "jow@email.com", retries: 0, status: "sent"},
    %{email: "cattani@mail.com", retries: 2, status: "failed"}
  ],
  max_retries: 2
}

# Stop the GenServer
iex> GenServer.stop pid
Terminating with reason normal
:ok
```