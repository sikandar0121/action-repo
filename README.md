# action-repo
Repo to handle GitHub actions with webhook
<!DOCTYPE html>
<html>
<head>
  <title>GitHub Events UI</title>
</head>
<body>
  <h2>GitHub Latest Events</h2>
  <ul id="events-list"></ul>

  <script>
    async function fetchEvents() {
      const response = await fetch('http://your_server_ip:5000/get-events');
      const data = await response.json();

      const list = document.getElementById("events-list");
      list.innerHTML = "";

      data.forEach(event => {
        let text = "";
        if (event.event_type === "push") {
          text = `${event.author} pushed to ${event.to_branch} on ${new Date(event.timestamp).toUTCString()}`;
        } else if (event.event_type === "pull_request") {
          text = `${event.author} submitted a pull request from ${event.from_branch} to ${event.to_branch} on ${new Date(event.timestamp).toUTCString()}`;
        } else if (event.event_type === "merge") {
          text = `${event.author} merged branch ${event.from_branch} to ${event.to_branch} on ${new Date(event.timestamp).toUTCString()}`;
        }
        const li = document.createElement("li");
        li.textContent = text;
        list.appendChild(li);
      });
    }

    setInterval(fetchEvents, 15000);
    fetchEvents();
  </script>
</body>
</html>

