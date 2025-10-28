# Practical 01 &mdash; React Quiz Application

## Overview

This practical focuses on building a simple quiz application using **React**. The goal is to demonstrate fundamental concepts of React such as components, state management, props, and event handling, while also applying basic software testing and quality assurance principles.

**GitHub Repository:** [https://github.com/tsheringphuntsho18/reactquiz](https://github.com/tsheringphuntsho18/reactquiz)

---

## Objectives

- Develop a functional quiz application using React.
- Apply component-based architecture for modularity and reusability.
- Implement state management for quiz logic (questions, answers, scoring).
- Practice basic testing and debugging techniques.

---

## Implementation Steps

1. **Project Setup**
   - Initialized a new React project using `create-react-app`.
   - Set up the project structure with separate folders for components and assets.

2. **Component Design**
   - Created reusable components such as `Quiz`, `Question`, `Options`, and `Score`.
   - Used props to pass data between components.

3. **State Management**
   - Utilized React's `useState` hook to manage current question, selected answers, and score.
   - Implemented logic to handle user interactions and update state accordingly.

4. **User Interface**
   - Designed a clean and intuitive UI for the quiz using CSS.
   - Added images and assets for better user experience (see [assets/](assets/) folder).

5. **Testing & Debugging**
   - Performed manual testing to ensure correct functionality.
   - Debugged issues related to state updates and component rendering.

---

## Screenshots

| Start Screen | Quiz in Progress | Score Report |
|--------------|------------------|-------------|
| ![Start](assets/start.png) | ![Quiz](assets/quiz.png) | ![Score](assets/score.png) |

---

## Challenges Faced

- Managing state transitions between questions and handling edge cases (e.g., last question).
- Ensuring components re-render correctly on state changes.
- Debugging issues with answer selection and score calculation.

---

## Conclusion

This practical provided hands-on experience with React fundamentals and reinforced the importance of component-based design and state management. The project also highlighted the need for thorough testing to ensure application reliability.

For full source code and further details, visit the [GitHub repository](https://github.com/tsheringphuntsho18/reactquiz).