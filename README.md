# Create-Quiz-Application-
import java.util.*;

public class QuizapplicationWithtimer {

    private static final int QUIZ_TIME_SECONDS = 20;
    private static int currentQuestionIndex = 0;
    private static int score = 0;

    private static final List<Question> questions = Arrays.asList(
            new Question("What is the capital of France?",
                    Arrays.asList("A. Berlin", "B. Madrid", "C. Paris", "D. Rome"), "C"),
            new Question("Which planet is known as the Red Planet?",
                    Arrays.asList("A. Venus", "B. Mars", "C. Jupiter", "D. Saturn"), "B"),
            new Question("What is the largest mammal in the world?",
                    Arrays.asList("A. Elephant", "B. Blue Whale", "C. Giraffe", "D. Dolphin"), "B")
    );

    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);

        Timer timer = new Timer();

        while (currentQuestionIndex < questions.size()) {
            Question currentQuestion = questions.get(currentQuestionIndex);

            System.out.println("\nQuestion " + (currentQuestionIndex + 1) + ": " + currentQuestion.getQuestion());
            for (String option : currentQuestion.getOptions()) {
                System.out.println(option);
            }

            QuestionTimer questionTimer = new QuestionTimer();
            timer.schedule(questionTimer, 0, 1000 * QUIZ_TIME_SECONDS);

            System.out.print("Your answer: ");
            String userAnswer = scanner.nextLine();

            questionTimer.cancel();

           
            if (userAnswer.equalsIgnoreCase(currentQuestion.getCorrectAnswer())) {
                System.out.println("Correct!");
                score++;
            } else {
                System.out.println("Incorrect. The correct answer is: " + currentQuestion.getCorrectAnswer());
            }

            currentQuestionIndex++;
        }

    
        System.out.println("\nQuiz completed!");
        System.out.println("Your score: " + score + "/" + questions.size());

        timer.cancel();

        scanner.close();
    }

    static class Question {
        private String question;
        private List<String> options;
        private String correctAnswer;

        public Question(String question, List<String> options, String correctAnswer) {
            this.question = question;
            this.options = options;
            this.correctAnswer = correctAnswer;
        }

        public String getQuestion() {
            return question;
        }

        public List<String> getOptions() {
            return options;
        }

        public String getCorrectAnswer() {
            return correctAnswer;
        }
    }

    static class QuestionTimer extends TimerTask {
        
        public void run() {
            System.out.println("Time's up! Moving to the next question.");
            currentQuestionIndex++;
            this.cancel(); 
        }
    }
}
