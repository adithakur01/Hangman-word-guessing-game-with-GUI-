import tkinter as tk
import random

# List of words for the game
words = [
    "hangman", "python", "program", "coding", "computer", "gaming","keyboard", "mouse", "monitor", "software",
    "hardware", "algorithm", "developer", "database", "internet", "application", "network", "security", "website",
    "interface", "function", "variable", "loop", "conditional", "framework", "machine", "learning", "artificial",
    "intelligence", "encryption", "authentication", "authorization", "server", "client", "protocol", "compiler",
    "operating", "system", "debugging", "testing", "iteration", "optimization", "code", "variable", "array", "list",
    "tuple", "dictionary", "class", "object", "inheritance", "polymorphism", "abstraction", "encapsulation",
    "interface", "module", "library", "framework", "interface", "web", "desktop", "mobile", "responsive", "backend",
    "frontend", "fullstack", "user", "experience", "version", "control", "git", "repository", "branch", "merge",
    "conflict", "remote", "repository", "commit", "pull", "push", "fetch", "clone", "fork", "branching", "agile",
    "scrum", "kanban", "waterfall", "sprint", "iteration", "backlog", "user", "story", "epic", "task", "estimate",
    "velocity", "burn", "down", "chart", "stand-up", "meeting", "retrospective", "product", "owner", "scrum",
    "master", "development", "team", "architecture", "design", "requirements", "specification", "analysis",
    "documentation", "diagram", "UML", "flowchart", "ER", "database", "diagram", "sequence", "diagram", "use",
    "case", "diagram", "activity", "diagram", "component", "diagram", "deployment", "diagram", "coding", "standard",
    "best", "practice", "debugging", "refactoring", "performance", "optimization", "code", "review", "integration",
    "testing", "unit", "testing", "integration", "testing", "system", "testing", "acceptance", "testing", "automation",
    "CI/CD", "pipeline", "continuous", "integration", "continuous", "deployment", "devops", "containerization",
    "Docker", "orchestration", "Kubernetes", "monitoring", "logging", "alerting", "scaling", "load", "balancing",
    "cloud", "computing", "serverless", "microservices", "RESTful", "API", "SOAP", "API", "GraphQL", "JSON", "XML",
    "HTTP", "HTTPS", "TCP", "IP", "DNS", "URL", "URI", "UX", "UI", "responsive", "design", "mockup", "prototype",
]

# Function to select a random word from the list
def choose_word():
    return random.choice(words)

class HangmanGame:
    def __init__(self, root):
        self.root = root
        self.root.title("Hangman Game")

        self.secret_word = choose_word().lower()
        self.guess_word = ['_' if char.isalpha() else char for char in self.secret_word]

        self.attempts = 6
        self.guessed_letters = []

        self.create_widgets()
        self.update_display()

    def create_widgets(self):
        self.secret_word_label = tk.Label(self.root, text=' '.join(self.guess_word), font=('Arial', 18))
        self.secret_word_label.pack()

        self.guess_label = tk.Label(self.root, text="Enter a letter:", font=('Arial', 12))
        self.guess_label.pack()

        self.guess_entry = tk.Entry(self.root, font=('Arial', 12))
        self.guess_entry.pack()

        self.submit_button = tk.Button(self.root, text="Guess", command=self.check_guess)
        self.submit_button.pack()

        self.message_label = tk.Label(self.root, text="", font=('Arial', 12))
        self.message_label.pack()

    def update_display(self):
        self.secret_word_label.config(text=' '.join(self.guess_word))
        self.message_label.config(text=f"Attempts left: {self.attempts}")

    def check_guess(self):
        guess = self.guess_entry.get().lower()
        self.guess_entry.delete(0, tk.END)

        if len(guess) != 1 or not guess.isalpha():
            self.message_label.config(text="Please enter a single letter.")
            return

        if guess in self.guessed_letters:
            self.message_label.config(text="You've already guessed this letter.")
            return

        self.guessed_letters.append(guess)

        if guess in self.secret_word:
            for i, letter in enumerate(self.secret_word):
                if letter == guess:
                    self.guess_word[i] = guess
        else:
            self.attempts -= 1

        self.update_display()
        self.check_game_over()

    def check_game_over(self):
        if '_' not in self.guess_word:
            self.message_label.config(text="Congratulations! You've guessed the word.")
            self.submit_button.config(state=tk.DISABLED)
        elif self.attempts == 0:
            self.message_label.config(text=f"Game over. The word was '{self.secret_word}'.")
            self.submit_button.config(state=tk.DISABLED)

def main():
    root = tk.Tk()
    hangman_game = HangmanGame(root)
    root.mainloop()

if __name__ == "__main__":
    main()
