# Continuous UI Testing Loop

This file governs the automated, iterative testing cycle for the application's user interface. Follow this loop sequentially, analyzing and verifying one UI workflow or user story at a time.

---

## The Master Loop Control

1. **DISCOVER:** Scan the file `TODO.md` and `docs/features.md` and identify the **highest-priority user flow or UI feature** that haven't been previously tested in the current session.
2. **PLAN:** Map out the exact visual navigation path, expected behaviors, element interactions, and critical data inputs needed to test the target UI feature. You can group correlated features together to test them all in one shot.
3. **EXECUTE:** Spin up a Chrome (headless prefereably) and interact with the live UI environment to run the test flow. Observe layout rendering, state transitions, and responsive behavior. Don't run unit, integration, or e2e test suites, do real world live browser-based tests.
4. **RECURRENT CHECKS:** Immediately pass the testing session and its results through the **Recurrent UI Validation Checklist** below.
5. **REPORT & REPEAT:** Document any bugs, visual defects, or functional failures in the `TODO.md `of the target project (under Bugs to fix section). If the test passes, log the successful execution run. Move to the next task by restarting the loop at Step 1.

---

## Recurrent UI Validation Checklist

*Apply these 6 phases to the tested UI workflow BEFORE marking a test case as successfully verified.*

### Phase 1: Functional & Interaction Integrity

* [ ] **Happy Path Validation:** Confirm that the primary user goal (e.g., successful checkout, form submission) can be completed without roadblocks.
* [ ] **State Persistence:** Verify that UI states change correctly upon user interaction (e.g., loaders appear during API calls, success toasts pop up, modals open/close smoothly).
* [ ] **Navigation & Links:** Ensure all links, buttons, breadcrumbs, and redirect paths within the flow point to the correct destinations.
* [ ] **Input Field Stress Test:** Test input boundaries using empty states, excessively long text, and special characters to ensure fields handle them gracefully without breaking the layout.

### Phase 2: Visual & Design Fidelity

* [ ] **Layout & Alignment:** Verify that text, buttons, images, and containers are perfectly aligned according to standard design spacing. Check for overlapping elements.
* [ ] **Responsive Scaling:** Test the UI at multiple viewport sizes (Mobile, Tablet, Desktop). Ensure elements scale fluidly, text doesn't clip, and the hamburger menu works correctly on smaller screens.
* [ ] **Typography & Asset Quality:** Check that fonts render correctly, no placeholder text (like "Lorem Ipsum") remains, and images or icons are not broken or pixelated.
* [ ] **Dark/Light Theme Check:** If applicable, toggle themes mid-flow to verify that colors, text contrast, and backgrounds switch flawlessly.

### Phase 3: Error Handling & Resilience

* [ ] **Error Message Clarity:** Trigger intentional failures (e.g., submitting an invalid email, leaving required fields blank). Confirm that helpful, clear error messages appear.
* [ ] **Inline vs. Global Validation:** Ensure error messages are anchored to the correct fields and do not block other critical UI components.
* [ ] **Recovery Paths:** Verify that the UI allows the user to easily fix their error and resubmit without needing a hard page refresh.

### Phase 4: Accessibility (a11y) & Usability

* [ ] **Keyboard Navigation:** Verify that the entire flow can be navigated using only the `Tab`, `Enter`, `Space`, and arrow keys.
* [ ] **Focus Indicators:** Ensure that a clear visual outline or indicator follows the active element during keyboard navigation.
* [ ] **Contrast & Readability:** Confirm that text colors provide enough contrast against backgrounds to be easily readable.

### Phase 5: Performance & Perception

* [ ] **Perceived Speed:** Audit the flow for sluggish transitions, frozen elements, or delayed UI rendering.
* [ ] **Loading States:** Verify that skeletons, spinners, or progress bars appear during network-heavy interactions so the application never feels broken or dead.

### Phase 6: Cross-Browser & Environment Consistency

* [ ] **Browser Parity:** Run the identical flow across different browser engines (Chromium, WebKit, Firefox) to catch engine-specific visual discrepancies.
* [ ] **Regression Check:** Confirm that executing this test flow didn't inadvertently disrupt or alter the UI of neighboring features or global elements (like the nav bar or footer).
