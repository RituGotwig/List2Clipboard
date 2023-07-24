# copy2clipboard
Easy tool to copy multiple data entries from data sheets or through manual inputs and pasting one input at a time on webpages (Eg. useful in time tracking tools, statistical data entries etc.)

#Step1: Copy the code below

```

let abortListener = new AbortController();

introduction();
function introduction() {
  addNode();
  let selection = null;
  selection = prompt(
    `𝙒𝙚𝙡𝙘𝙤𝙢𝙚 𝙩𝙤  📚𝘼𝙧𝙧𝙖𝙮 2️⃣𝘾𝙡𝙞𝙥𝙗𝙤𝙖𝙧𝙙 📋
Iterate trough your text lists with your clipboard like a Pro!

𝐏𝐥𝐞𝐚𝐬𝐞 𝐜𝐡𝐨𝐨𝐬𝐞 𝐭𝐡𝐞 𝐢𝐧𝐩𝐮𝐭 𝐭𝐲𝐩𝐞 𝐨𝐟 𝐲𝐨𝐮𝐫 𝐭𝐞𝐱𝐭 𝐥𝐢𝐬𝐭

Automatic mode (automatic): auto-detect input seperator

Manual Seperator Mode:
Enter 'E' for excel sheet row/column (only 1D).
Enter 'C' for comma separated values (CSV).

For any other seperator: just specify, e.g. '-'`,
    "automatic"
  );
  if (!selection) {
    return;
  } else {
    selection = selection.toLowerCase();

    if (selection == "c") {
      inputPrompt("csv");
    } else if (selection == "e") {
      inputPrompt("excel");
    } else {
      inputPrompt(selection);
    }
  }
}

function automaticDetection(input) {
  if (input.includes(",")) {
    return ",";
  }
  return excelCase(input);
}

function excelCase(input) {
  if (input.includes("\n")) {
    return "\n";
  }
  if (input.includes("\t")) {
    return "\t";
  } else {
    return false;
  }
}

function inputPrompt(seperatorType) {
  const inputPromptTexts = {
    automatic: "Please enter your text list. We auto recognize the seperator",
    csv: "Please enter your array in CSV eg: 1, 2, 3...",
    excel: "Please enter one row or column values from excel sheet",
    custom: "Please enter your array with your custom seperator",
  };

  let input = prompt(inputPromptTexts[seperatorType]);

  if (!input) {
    introduction();
    return;
  } else {
    if (seperatorType === "automatic") {
      let detectionResult = this.automaticDetection(input);
      if (detectionResult) {
        seperatorType = detectionResult;
      } else {
        alert(
          "Sorry, we could not recognize the seperator. Please specify the seperator manualy."
        );
        this.selectionCheck();
        return;
      }
    } else {
      if (seperatorType === "csv") {
        seperatorType = ",";
      }

      if (seperatorType === "excel") {
        seperatorType = excelCase(input);
      }
    }

    let clipboardArray = input.split(seperatorType);
    if (clipboardArray.length <= 1) {
      alert("Invalid input, please try again");
      checkCSV();
    } else {
      alert(`Your values are ${clipboardArray} `);
      let index = 0;

      copy(clipboardArray[index]);
      this.copy = copy;

      addEventListener(
        "paste",
        () => {
          index++;
          if (index > clipboardArray.length) {

            copy("");
            const newData = confirm(
              "You reached the end of the array.\nDo you want to enter new data?"
            );
            leaveScript()

            if (newData) {

              this.input = null;

              this.clipboardArray = null;
              index = 0

              introduction();

            }
            return;

          }
          copy(clipboardArray[index]);
        },
        { signal: abortListener.signal }
      );

      addEventListener(
        "copy",
        () => {
          alert(
            "You copied new content into the clipboard - Stopping Array2Clipboard"
          );
          leaveScript()
        },
        { signal: abortListener.signal }
      );
    }
  }
}

function leaveScript() {
  abortListener.abort();
  document.getElementById("array2clipboard").remove();
  abortListener = new AbortController();
}

function addNode() {
  const node = document.createElement("p");
  node.id = "array2clipboard";
  node.style = 'background: black;color: white;padding: 4px 10px;border: 3px solid red'
  const textnode = document.createTextNode("Pasting from Array2Clipboard. Copy new content to stop script.");
  node.appendChild(textnode);
  document.body.prepend(node);
}
```
#Step 2: Right click on the webpage where you want to paste the contents, select Inspect and go to Console tab
#Step 3: Paste the code --> enter

A pop-up should show up with instructions to enter your data. 

#Important to note:
- The tool accesses your clipboard
- You can paste the copied elements using Ctrl+v or paste, and your elements are pasted in a series
- You can use it for a single row or column of data at a time, for the next row just go for the new data option in the end
- The tool aborts once you copy some other element from your web page while using it
