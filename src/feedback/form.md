<!-- src/feedback.md -->

# Feedback

We appreciate your feedback! Please fill out the form below.

<form id="feedback-form">
    <textarea name="feedback" placeholder="Enter your feedback" rows="4" cols="50"></textarea><br>
    <button type="submit">Submit Feedback</button>
</form>

<script>
    document.getElementById('feedback-form').onsubmit = async function(e) {
        e.preventDefault();
        const feedback = document.querySelector('[name="feedback"]').value;
        const response = await fetch('http://127.0.0.1:8080/feedback', {
            method: 'POST',
            body: feedback
        });
        if (response.ok) {
            alert('Feedback submitted successfully');
        } else {
            alert('Error submitting feedback');
        }
    };
</script>

