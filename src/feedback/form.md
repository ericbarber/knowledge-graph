<!-- src/feedback.md -->

# Feedback

We appreciate your feedback! Please fill out the form below.

<form id="feedback-form">
    <textarea name="feedback" placeholder="Enter your feedback" rows="4" cols="50"></textarea><br>
    <button type="submit">Submit Feedback</button>
</form>

<div id="feedback-message"></div> <!-- Added a div for feedback messages -->

<script>
    document.getElementById('feedback-form').onsubmit = async function(e) {
        e.preventDefault();
        const feedback = document.querySelector('[name="feedback"]').value;
        const messageElement = document.getElementById('feedback-message');

        // Display submitting status
        messageElement.textContent = 'Submitting...';
        messageElement.style.color = 'blue'; // Optional: Style for submitting message

        // Set a timeout duration (e.g., 5000 milliseconds)
        const timeoutDuration = 5000;

        // Create a promise that rejects after the timeout duration
        const timeout = new Promise((_, reject) => {
            setTimeout(() => {
                reject(new Error('Request timed out'));
            }, timeoutDuration);
        });

        // Race the fetch against the timeout
        try {
            const response = await Promise.race([
                // fetch('http://127.0.0.1:8080/feedback', {
                fetch('http://192.168.0.55:8080/feedback', {
                    method: 'POST',
                    body: feedback
                }),
                timeout
            ]);

            if (response.ok) {
                messageElement.textContent = 'Thank you for the feedback';
                messageElement.style.color = 'green';
            } else {
                throw new Error('Response not OK');
            }
        } catch (error) {
            messageElement.textContent = 'Feedback not received, please try again';
            messageElement.style.color = 'red';
        }
    };
</script>

