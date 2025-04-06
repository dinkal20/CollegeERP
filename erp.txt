import javafx.application.Application;
import javafx.beans.property.*;
import javafx.collections.FXCollections;
import javafx.collections.ObservableList;
import javafx.geometry.*;
import javafx.scene.Scene;
import javafx.scene.control.*;
import javafx.scene.control.cell.PropertyValueFactory;
import javafx.scene.layout.*;
import javafx.scene.paint.Color;
import javafx.scene.shape.Circle;
import javafx.scene.text.Font;
import javafx.scene.text.Text;
import javafx.stage.Modality;
import javafx.stage.Stage;
import javafx.stage.StageStyle;
import javafx.animation.KeyFrame;
import javafx.animation.Timeline;

import java.time.*;
import java.time.format.DateTimeFormatter;
import java.util.*;

public class Main extends Application {

    private Stage primaryStage;
    private final String PRIMARY_COLOR = "#3F51B5";
    private final String EMAIL = "rahi";
    private final String PASSWORD = "1234";

    @Override
    public void start(Stage primaryStage) {
        this.primaryStage = primaryStage;
        primaryStage.setTitle("Student Support System - Login");

        // Show login page first
        Scene loginScene = createLoginScene();
        primaryStage.setScene(loginScene);
        primaryStage.setResizable(false);
        primaryStage.show();

        

    }

    private Scene createLoginScene() {
        BorderPane root = new BorderPane();
        root.setStyle("-fx-background-color: linear-gradient(to bottom right, #3F51B5, #7986CB);");

        VBox loginBox = new VBox(20);
        loginBox.setAlignment(Pos.CENTER);
        loginBox.setPadding(new Insets(20));
        loginBox.setStyle("-fx-background-color: white; -fx-background-radius: 10; -fx-effect: dropshadow(gaussian, rgba(0,0,0,0.3), 10, 0, 0, 5);");
        loginBox.setMaxWidth(400);
        loginBox.setMaxHeight(500);

        Text title = new Text("    College ERP Login page\n\n\n\nusername:rahi,password:1234");
        title.setFont(Font.font("Arial", 28));
        title.setFill(Color.web(PRIMARY_COLOR));

        TextField emailField = new TextField();
        emailField.setPromptText("username");
        emailField.setStyle("-fx-font-size: 14px; -fx-padding: 10; -fx-background-radius: 5;");
        emailField.setMaxWidth(300);

        PasswordField passwordField = new PasswordField();
        passwordField.setPromptText("Password");
        passwordField.setStyle("-fx-font-size: 14px; -fx-padding: 10; -fx-background-radius: 5;");
        passwordField.setMaxWidth(300);

        Button loginButton = new Button("Login");
        loginButton.setStyle("-fx-background-color: " + PRIMARY_COLOR + "; -fx-text-fill: white; -fx-font-size: 16px; -fx-padding: 10 40; -fx-background-radius: 5;");
        loginButton.setMaxWidth(300);

        Label errorLabel = new Label("");
        errorLabel.setStyle("-fx-text-fill: red; -fx-font-size: 12px;");

        loginButton.setOnAction(e -> {
            String enteredEmail = emailField.getText().trim();
            String enteredPassword = passwordField.getText();
            if (enteredEmail.equals(EMAIL) && enteredPassword.equals(PASSWORD)) {
                // Successful login, switch to main app
                setupMainApp();
            } else {
                errorLabel.setText("Invalid email or password");
            }
        });

        loginBox.getChildren().addAll(title, emailField, passwordField, loginButton, errorLabel);
        root.setCenter(loginBox);

        return new Scene(root, 1000, 800);
    }

    private void setupMainApp() {
        primaryStage.setTitle("Student Support System");

        // Dashboard layout
        VBox dashboard = createDashboard();

        Scene scene = new Scene(dashboard, 1000, 800);
        scene.setFill(Color.LIGHTGRAY);
        primaryStage.setScene(scene);
    }

    private VBox createDashboard() {
        VBox dashboard = new VBox(0); // Reduced spacing
        dashboard.setStyle("-fx-background-color: #F5F5F5;");

        // Header
        VBox header = createHeader();

        // Icons container
        HBox iconsBox = new HBox(30);
        iconsBox.setAlignment(Pos.CENTER);
        iconsBox.setPadding(new Insets(10, 20, 20, 20)); // Reduced top padding

        // Icon buttons
        Button gpaButton = createIconButton("GPA Predictor", "📊");
        Button timetableButton = createIconButton("Timetable", "🕒");
        Button mentalHealthButton = createIconButton("Mental Health", "🧠");

        gpaButton.setOnAction(e -> showSection(createGPAPane()));
        timetableButton.setOnAction(e -> showSection(createTimetablePane()));
        mentalHealthButton.setOnAction(e -> showSection(createMentalHealthPane()));

        iconsBox.getChildren().addAll(gpaButton, timetableButton, mentalHealthButton);

        dashboard.getChildren().addAll(header, iconsBox);
        return dashboard;
    }

    private VBox createHeader() {
        VBox header = new VBox(5);
        header.setPadding(new Insets(30));
        header.setStyle("-fx-background-color: " + PRIMARY_COLOR + "; -fx-effect: dropshadow(gaussian, rgba(0,0,0,0.3), 10, 0, 0, 5);");

        // Title
        Text title = new Text("IAR ERP System");
        title.setFont(Font.font("Arial", 18));
        title.setFill(Color.WHITE);

        // Student Info - First Line
        HBox studentInfo1 = new HBox(10);
        Text name = new Text("A");
        name.setFont(Font.font("Arial", 16));
        name.setFill(Color.WHITE);
        Text contact = new Text("9000001212 | a*****@gmail.com");
        contact.setFont(Font.font("Arial", 12));
        contact.setFill(Color.WHITE);
        studentInfo1.getChildren().addAll(name, contact);
        studentInfo1.setAlignment(Pos.CENTER_LEFT);

        // Student Info - Second Line
        HBox studentInfo2 = new HBox(20);
        Text branch = new Text("Branch: B.Tech");
        Text sem = new Text("Sem: 4");
        Text division = new Text("Division: Btech-IT");
        Text rollNo = new Text("Roll.No.: 20");
        Text batch = new Text("Batch: A1");
        for (Text t : Arrays.asList(branch, sem, division, rollNo, batch)) {
            t.setFont(Font.font("Arial", 12));
            t.setFill(Color.WHITE);
        }
        studentInfo2.getChildren().addAll(branch, sem, division, rollNo, batch);
        studentInfo2.setAlignment(Pos.CENTER_LEFT);

        header.getChildren().addAll(title, studentInfo1, studentInfo2);
        return header;
    }

    private Button createIconButton(String text, String emoji) {
        Button button = new Button(emoji + "\n" + text);
        button.setStyle("-fx-background-color: white; -fx-background-radius: 10; -fx-effect: dropshadow(gaussian, rgba(0,0,0,0.2), 10, 0, 0, 5); " +
                "-fx-font-size: 16px; -fx-text-fill: " + PRIMARY_COLOR + "; -fx-padding: 20; -fx-alignment: center;");
        button.setPrefSize(150, 150);
        button.setWrapText(true);
        return button;
    }

    private void showSection(Pane content) {
        VBox layout = new VBox(0); // Reduced spacing
        layout.setStyle("-fx-background-color: #F5F5F5;");

        VBox header = createHeader(); // Reuse the same header
        Button backButton = new Button("Back to Dashboard");
        backButton.setStyle("-fx-background-color: #9E9E9E; -fx-text-fill: white; -fx-font-size: 14px; -fx-padding: 10 20; -fx-background-radius: 5;");
        backButton.setOnAction(e -> setupMainApp());

        HBox backButtonBox = new HBox(backButton);
        backButtonBox.setPadding(new Insets(10, 0, 10, 10)); // Reduced padding
        backButtonBox.setAlignment(Pos.TOP_LEFT);

        layout.getChildren().addAll(header, backButtonBox, content);
        Scene scene = new Scene(layout, 1000, 800);
        primaryStage.setScene(scene);
    }

    // GPA Predictor Pane
    private Pane createGPAPane() {
        BorderPane root = new BorderPane();

        ObservableList<Course> courses = FXCollections.observableArrayList();
        TableView<Course> tableView = new TableView<>();
        Label resultLabel = new Label("Your predicted GPA will appear here.");
        double[] totalCredits = {0};
        double[] totalGradePoints = {0};

        String[] courseNames = {"Computer Networks", "Database Management", "Software Engineering",
                "Operating Systems", "Web Development"};

        VBox inputBox = new VBox(15);
        inputBox.setPadding(new Insets(20));

        Label titleLabel = new Label("GPA Predictor");
        titleLabel.setStyle("-fx-font-size: 24px; -fx-font-weight: bold;");
        titleLabel.setTextFill(Color.web(PRIMARY_COLOR));
        inputBox.getChildren().add(titleLabel);

        GridPane courseInputGrid = new GridPane();
        courseInputGrid.setHgap(10);
        courseInputGrid.setVgap(10);

        TextField[] marksFields = new TextField[5];
        for (int i = 0; i < 5; i++) {
            Label marksLabel = new Label(courseNames[i] + " Marks (0-100):");
            TextField marksField = new TextField();
            marksFields[i] = marksField;
            courseInputGrid.add(marksLabel, 0, i);
            courseInputGrid.add(marksField, 1, i);
        }

        inputBox.getChildren().add(courseInputGrid);

        Button addMarksButton = new Button("Add Marks");
        addMarksButton.setStyle("-fx-background-color: #4CAF50; -fx-text-fill: white;");
        inputBox.getChildren().add(addMarksButton);

        tableView.getColumns().addAll(
                createTableColumn("Course Name", "courseName"),
                createTableColumn("Marks", "marks"),
                createTableColumn("Credits", "credits"),
                createTableColumn("Grade", "grade"),
                createTableColumn("Grade Points", "gradePoints")
        );

        tableView.setItems(courses);
        tableView.setPrefHeight(200);

        resultLabel.setStyle("-fx-font-size: 18px; -fx-font-weight: bold;");
        resultLabel.setTextFill(Color.web(PRIMARY_COLOR));

        root.setLeft(inputBox);
        root.setCenter(tableView);
        root.setBottom(resultLabel);
        BorderPane.setAlignment(resultLabel, Pos.CENTER);
        BorderPane.setMargin(resultLabel, new Insets(10));

        Random random = new Random();
        for (String courseName : courseNames) {
            double credits = 2 + random.nextInt(3);
            courses.add(new Course(courseName, 0, credits, "-", 0));
            totalCredits[0] += credits;
        }

        addMarksButton.setOnAction(e -> {
            totalGradePoints[0] = 0;
            try {
                for (int i = 0; i < 5; i++) {
                    int marks = Integer.parseInt(marksFields[i].getText());
                    Course course = courses.get(i);
                    String grade = getGrade(marks);
                    double gradePoints = getGradePoints(marks);
                    course.setMarks(marks);
                    course.setGrade(grade);
                    course.setGradePoints(gradePoints);
                    totalGradePoints[0] += gradePoints * course.getCredits();
                }
                double gpa = totalGradePoints[0] / totalCredits[0];
                resultLabel.setText("Your predicted GPA: " + String.format("%.2f", gpa));
            } catch (NumberFormatException ex) {
                showAlert("Invalid Input", "Please enter valid numbers for marks.");
            }
        });

        return root;
    }

    // Timetable Pane
    private Pane createTimetablePane() {
        BorderPane root = new BorderPane();

        List<Lecture> lectures = new ArrayList<>();
        TableView<Lecture> lectureTable = new TableView<>();
        TableView<Lecture> cancelledTable = new TableView<>();

        VBox inputPanel = new VBox(10);
        inputPanel.setPadding(new Insets(15));

        HBox inputRow = new HBox(10);
        TextField nameField = new TextField();
        nameField.setPromptText("Lecture Name");
        ComboBox<String> dayCombo = new ComboBox<>(FXCollections.observableArrayList(
                "Monday", "Tuesday", "Wednesday", "Thursday", "Friday", "Saturday", "Sunday"));
        TextField timeField = new TextField();
        timeField.setPromptText("HH:mm");
        Button addButton = new Button("Add");
        addButton.setStyle("-fx-background-color: #4CAF50; -fx-text-fill: white;");

        inputRow.getChildren().addAll(nameField, dayCombo, timeField, addButton);
        inputPanel.getChildren().add(inputRow);

        lectureTable.getColumns().addAll(
                createTableColumn("Lecture Name", "name"),
                createTableColumn("Day", "day"),
                createTableColumn("Time", "time")
        );

        cancelledTable.getColumns().addAll(
                createTableColumn("Lecture Name", "name"),
                createTableColumn("Day", "day"),
                createTableColumn("Time", "time")
        );

        Button cancelButton = new Button("Cancel Selected");
        cancelButton.setStyle("-fx-background-color: #DC3545; -fx-text-fill: white;");
        inputPanel.getChildren().add(cancelButton);

        TabPane timetableTabs = new TabPane();
        Tab activeTab = new Tab("Active Lectures");
        Tab cancelledTab = new Tab("Cancelled Lectures");
        activeTab.setContent(new ScrollPane(lectureTable));
        cancelledTab.setContent(new ScrollPane(cancelledTable));
        timetableTabs.getTabs().addAll(activeTab, cancelledTab);

        root.setTop(inputPanel);
        root.setCenter(timetableTabs);

        addButton.setOnAction(e -> {
            try {
                String name = nameField.getText().trim();
                String day = dayCombo.getValue();
                LocalTime time = LocalTime.parse(timeField.getText().trim(), DateTimeFormatter.ofPattern("HH:mm"));
                if (!name.isEmpty() && day != null) {
                    lectures.add(new Lecture(name, DayOfWeek.valueOf(day.toUpperCase()), time));
                    updateTimetableTables(lectures, lectureTable, cancelledTable);
                    nameField.clear();
                    timeField.clear();
                }
            } catch (Exception ex) {
                showAlert("Error", "Invalid time format (use HH:mm)");
            }
        });

        cancelButton.setOnAction(e -> {
            Lecture selected = lectureTable.getSelectionModel().getSelectedItem();
            if (selected != null) {
                selected.setCancelled(true);
                updateTimetableTables(lectures, lectureTable, cancelledTable);
            }
        });

        Timeline notificationTimer = new Timeline(new KeyFrame(javafx.util.Duration.seconds(30), e ->
                checkNotifications(lectures)));
        notificationTimer.setCycleCount(Timeline.INDEFINITE);
        notificationTimer.play();

        return root;
    }

    // Mental Health Pane
    private Pane createMentalHealthPane() {
        StackPane root = new StackPane();
        VBox mainMenu = createMentalHealthMenu(root);
        root.getChildren().setAll(mainMenu);
        return root;
    }

    private VBox createMentalHealthMenu(StackPane root) {
        VBox menu = new VBox(20);
        menu.setAlignment(Pos.CENTER);
        menu.setPadding(new Insets(30));
        menu.setStyle("-fx-background-color: #F5F5F5;");

        Text title = new Text("Mental Health Support");
        title.setFont(Font.font(24));
        title.setStyle("-fx-fill: " + PRIMARY_COLOR + ";");

        Button moodButton = createFeatureButton("Mood Tracker");
        Button breathingButton = createFeatureButton("Breathing Exercise");
        Button chatButton = createFeatureButton("Chat Support");

        moodButton.setOnAction(e -> showMoodTracker(root, menu));
        breathingButton.setOnAction(e -> showBreathingExercise(root, menu));
        chatButton.setOnAction(e -> showChatSupport(root, menu));

        menu.getChildren().addAll(title, moodButton, breathingButton, chatButton);
        return menu;
    }

    private void showMoodTracker(StackPane root, VBox menu) {
        VBox layout = new VBox(15);
        layout.setAlignment(Pos.CENTER);
        layout.setPadding(new Insets(20));
        layout.setStyle("-fx-background-color: #FFFFFF;");

        Text title = new Text("How are you feeling?");
        title.setFont(Font.font(20));
        title.setStyle("-fx-fill: " + PRIMARY_COLOR + ";");

        HBox moodButtons = new HBox(15);
        moodButtons.setAlignment(Pos.CENTER);

        Button happyBtn = createMoodButton("Happy", "😊", "#FFF9C4");
        Button sadBtn = createMoodButton("Sad", "😢", "#E1BEE7");
        Button stressedBtn = createMoodButton("Stressed", "😫", "#BBDEFB");

        moodButtons.getChildren().addAll(happyBtn, sadBtn, stressedBtn);

        Text affirmation = new Text("Select your mood");
        affirmation.setFont(Font.font(14));
        affirmation.setWrappingWidth(350);

        happyBtn.setOnAction(e -> {
            layout.setStyle("-fx-background-color: #FFF9C4;");
            affirmation.setText(getAffirmation("Happy"));
        });

        sadBtn.setOnAction(e -> {
            layout.setStyle("-fx-background-color: #E1BEE7;");
            affirmation.setText(getAffirmation("Sad"));
        });

        stressedBtn.setOnAction(e -> {
            layout.setStyle("-fx-background-color: #BBDEFB;");
            affirmation.setText(getAffirmation("Stressed"));
        });

        Button backButton = createBackButton(root, menu);
        layout.getChildren().addAll(title, moodButtons, affirmation, backButton);
        root.getChildren().setAll(layout);
    }

    private void showBreathingExercise(StackPane root, VBox menu) {
        VBox layout = new VBox(20);
        layout.setAlignment(Pos.CENTER);
        layout.setPadding(new Insets(30));
        layout.setStyle("-fx-background-color: #E3F2FD;");

        Text title = new Text("Guided Breathing");
        title.setFont(Font.font(20));
        title.setStyle("-fx-fill: " + PRIMARY_COLOR + ";");

        Circle circle = new Circle(50, Color.web(PRIMARY_COLOR));
        Text instruction = new Text("Press Start to begin");
        instruction.setFont(Font.font(16));

        StackPane animationPane = new StackPane(circle, instruction);

        Button startButton = createFeatureButton("Start");
        Button stopButton = createFeatureButton("Pause");
        stopButton.setDisable(true);

        HBox controls = new HBox(15, startButton, stopButton);
        controls.setAlignment(Pos.CENTER);

        Timeline timeline = new Timeline(
                new KeyFrame(javafx.util.Duration.seconds(3), e -> {
                    circle.setRadius(80);
                    instruction.setText("Breathe IN...");
                }),
                new KeyFrame(javafx.util.Duration.seconds(6), e -> {
                    circle.setRadius(50);
                    instruction.setText("Breathe OUT...");
                })
        );
        timeline.setCycleCount(Timeline.INDEFINITE);

        startButton.setOnAction(e -> {
            timeline.play();
            startButton.setDisable(true);
            stopButton.setDisable(false);
        });

        stopButton.setOnAction(e -> {
            timeline.pause();
            startButton.setDisable(false);
            stopButton.setDisable(true);
            instruction.setText("Paused");
        });

        Button backButton = createBackButton(root, menu);
        layout.getChildren().addAll(title, animationPane, controls, backButton);
        root.getChildren().setAll(layout);
    }

    private void showChatSupport(StackPane root, VBox menu) {
        VBox layout = new VBox(10);
        layout.setPadding(new Insets(20));
        layout.setStyle("-fx-background-color: #ECEFF1;");

        Text title = new Text("Chat Support");
        title.setFont(Font.font(20));
        title.setStyle("-fx-fill: " + PRIMARY_COLOR + ";");

        TextArea chatArea = new TextArea("Support: Hello! How can I help you today?\n");
        chatArea.setEditable(false);
        chatArea.setWrapText(true);
        chatArea.setPrefHeight(250);

        TextField inputField = new TextField();
        inputField.setPromptText("Type your message...");

        Button sendButton = createFeatureButton("Send"); // Fixed: Use createFeatureButton instead of FeatureButton
        sendButton.setStyle("-fx-background-color: #4CAF50; -fx-text-fill: white;"); // Override style as before

        HBox inputArea = new HBox(10, inputField, sendButton);
        HBox.setHgrow(inputField, Priority.ALWAYS);

        sendButton.setOnAction(e -> {
            String message = inputField.getText().trim();
            if (!message.isEmpty()) {
                chatArea.appendText("You: " + message + "\n");
                inputField.clear();
                new Timeline(new KeyFrame(javafx.util.Duration.seconds(1), e2 ->
                        chatArea.appendText("Support: " + getChatResponse(message) + "\n"))).play();
            }
        });

        Button backButton = createBackButton(root, menu);
        layout.getChildren().addAll(title, chatArea, inputArea, backButton);
        root.getChildren().setAll(layout);
    }

    // Utility Methods
    private <S, T> TableColumn<S, T> createTableColumn(String title, String property) {
        TableColumn<S, T> column = new TableColumn<>(title);
        column.setCellValueFactory(new PropertyValueFactory<>(property));
        return column;
    }

    private String getGrade(int marks) {
        if (marks >= 90) return "O";
        if (marks >= 80) return "A+";
        if (marks >= 70) return "A";
        if (marks >= 60) return "B+";
        if (marks >= 50) return "B";
        if (marks >= 40) return "C";
        return "F";
    }

    private double getGradePoints(int marks) {
        if (marks >= 90) return 10;
        if (marks >= 80) return 9;
        if (marks >= 70) return 8;
        if (marks >= 60) return 7;
        if (marks >= 50) return 6;
        if (marks >= 40) return 5;
        return 0;
    }

    private Button createFeatureButton(String text) {
        Button button = new Button(text);
        button.setStyle("-fx-background-color: " + PRIMARY_COLOR + "; " +
                "-fx-text-fill: white; " +
                "-fx-font-size: 14px; " +
                "-fx-padding: 10 25; " +
                "-fx-background-radius: 5;");
        button.setMaxWidth(200);
        return button;
    }

    private Button createMoodButton(String text, String emoji, String color) {
        Button button = new Button(text + " " + emoji);
        button.setStyle("-fx-background-color: " + color + "; " +
                "-fx-text-fill: #333333; " +
                "-fx-font-size: 14px; " +
                "-fx-padding: 10 15; " +
                "-fx-background-radius: 5;");
        return button;
    }

    private String getAffirmation(String mood) {
        switch(mood) {
            case "Happy": return "You're shining today! Keep spreading positivity! ✨";
            case "Sad": return "It's okay to feel this way. Brighter days are coming. 💛";
            case "Stressed": return "Take a deep breath. You've overcome challenges before. 🌟";
            default: return "You're doing great!";
        }
    }

    private String getChatResponse(String message) {
        String lowerMsg = message.toLowerCase();
        if (lowerMsg.contains("stress") || lowerMsg.contains("anxious"))
            return "I understand. Would you like to try a breathing exercise?";
        if (lowerMsg.contains("sad") || lowerMsg.contains("depress"))
            return "I'm here for you. Remember you're not alone. 💛";
        if (lowerMsg.contains("happy") || lowerMsg.contains("good"))
            return "That's wonderful to hear! 😊";
        return "I'm listening. Tell me more about how you're feeling.";
    }

    private Button createBackButton(StackPane root, VBox menu) {
        Button button = createFeatureButton("Back to Menu");
        button.setStyle("-fx-background-color: #9E9E9E; -fx-text-fill: white;");
        button.setOnAction(e -> root.getChildren().setAll(menu));
        return button;
    }

    private void updateTimetableTables(List<Lecture> lectures, TableView<Lecture> lectureTable,
                                       TableView<Lecture> cancelledTable) {
        ObservableList<Lecture> activeLectures = FXCollections.observableArrayList();
        ObservableList<Lecture> cancelledLectures = FXCollections.observableArrayList();

        for (Lecture lecture : lectures) {
            if (lecture.isCancelled()) {
                cancelledLectures.add(lecture);
            } else {
                activeLectures.add(lecture);
            }
        }

        lectureTable.setItems(activeLectures);
        cancelledTable.setItems(cancelledLectures);
    }

    private void checkNotifications(List<Lecture> lectures) {
        LocalDateTime now = LocalDateTime.now();
        DayOfWeek currentDay = now.getDayOfWeek();
        LocalTime currentTime = now.toLocalTime();

        for (Lecture lecture : lectures) {
            if (!lecture.getDay().equals(currentDay)) continue;
            long secondsUntil = Duration.between(currentTime, lecture.getTime()).getSeconds();

            if (lecture.isCancelled()) {
                if (secondsUntil <= 1800 && secondsUntil > 0 && !lecture.isNotified()) {
                    showAlert("Cancelled Lecture",
                            "Reminder: \"" + lecture.getName() + "\" was cancelled (30 min notice)");
                    lecture.setNotified(true);
                }
            } else {
                if (secondsUntil <= 600 && secondsUntil > 0 && !lecture.isNotified()) {
                    showAlert("Lecture Reminder",
                            "Reminder: \"" + lecture.getName() + "\" starts in 10 minutes!");
                    lecture.setNotified(true);
                }
            }
            if (currentTime.isAfter(lecture.getTime())) {
                lecture.setNotified(false);
            }
        }
    }

    private void showAlert(String title, String message) {
        Alert alert = new Alert(Alert.AlertType.INFORMATION);
        alert.setTitle(title);
        alert.setHeaderText(null);
        alert.setContentText(message);
        alert.show();
    }

    public static void main(String[] args) {
        launch(args);
    }

    // Data Classes
    public static class Course {
        private final StringProperty courseName = new SimpleStringProperty();
        private final IntegerProperty marks = new SimpleIntegerProperty();
        private final DoubleProperty credits = new SimpleDoubleProperty();
        private final StringProperty grade = new SimpleStringProperty();
        private final DoubleProperty gradePoints = new SimpleDoubleProperty();

        public Course(String courseName, int marks, double credits, String grade, double gradePoints) {
            this.courseName.set(courseName);
            this.marks.set(marks);
            this.credits.set(credits);
            this.grade.set(grade);
            this.gradePoints.set(gradePoints);
        }

        public String getCourseName() { return courseName.get(); }
        public int getMarks() { return marks.get(); }
        public double getCredits() { return credits.get(); }
        public String getGrade() { return grade.get(); }
        public double getGradePoints() { return gradePoints.get(); }

        public void setMarks(int marks) { this.marks.set(marks); }
        public void setGrade(String grade) { this.grade.set(grade); }
        public void setGradePoints(double points) { this.gradePoints.set(points); }

        public StringProperty courseNameProperty() { return courseName; }
        public IntegerProperty marksProperty() { return marks; }
        public DoubleProperty creditsProperty() { return credits; }
        public StringProperty gradeProperty() { return grade; }
        public DoubleProperty gradePointsProperty() { return gradePoints; }
    }

    public static class Lecture {
        private final StringProperty name = new SimpleStringProperty();
        private final ObjectProperty<DayOfWeek> day = new SimpleObjectProperty<>();
        private final ObjectProperty<LocalTime> time = new SimpleObjectProperty<>();
        private final BooleanProperty notified = new SimpleBooleanProperty(false);
        private final BooleanProperty cancelled = new SimpleBooleanProperty(false);

        public Lecture(String name, DayOfWeek day, LocalTime time) {
            this.name.set(name);
            this.day.set(day);
            this.time.set(time);
        }

        public String getName() { return name.get(); }
        public DayOfWeek getDay() { return day.get(); }
        public LocalTime getTime() { return time.get(); }
        public boolean isNotified() { return notified.get(); }
        public boolean isCancelled() { return cancelled.get(); }

        public void setNotified(boolean value) { notified.set(value); }
        public void setCancelled(boolean value) { cancelled.set(value); }

        public StringProperty nameProperty() { return name; }
        public ObjectProperty<DayOfWeek> dayProperty() { return day; }
        public ObjectProperty<LocalTime> timeProperty() { return time; }
    }
}