### Typed mocks

```javascript
import "@testing-library/jest-dom";
import React from "react";
import renderer, {
  ReactTestInstance,
  ReactTestRenderer,
} from "react-test-renderer";
import Testable from "@app/infrastructure/testing/Testable";
import { Modal, IconButton } from "@mui/material";
import { detectAnyAdblocker } from "just-detect-adblock";
import AdblockDetector from "../AdblockDetector";
import { timeout } from "@app/utils/index";

jest.mock("just-detect-adblock");
const mockDetectAnyAdblocker = detectAnyAdblocker as jest.MockedFunction<
  typeof detectAnyAdblocker
>;

describe("AdblockDetector", () => {
  function createSut(): JSX.Element {
    return (
      <Testable>
        <AdblockDetector />
      </Testable>
    );
  }

  it("should render correctly", () => {
    const sut = createSut();
    expect(sut).toMatchSnapshot();
  });

  it("should not show adblock modal when detectAnyAdblocker returns false", async () => {
    console.log("detectAnyAdblocker", detectAnyAdblocker);
    mockDetectAnyAdblocker.mockReturnValue(Promise.resolve(false));
    const sut = createSut();
    const tree = renderer.create(sut).root;
    const modals = tree.findAllByType(Modal);
    expect(modals).toHaveLength(1);
    expect(modals[0].props.open).toBe(false);
    expect(tree.findAllByType("ModalContent")).toHaveLength(0);
  });
});
```
