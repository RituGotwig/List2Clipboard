# List2Clipboard
Easy Javascript tool which allows to copy multiple data entries into a virtual clipboard, and loops trough it one at a time with each paste.
Useful for things like mass-import of excell stats into forms, timesheets, etc.

- The tool accesses your clipboard
- You can paste the copied elements using Ctrl+v or paste, and your elements are pasted in a series
- You can use it for a single row or column of data at a time, for the next row just go for the new data option in the end
- The tool aborts if new text is copied into the clipboard

## Instructions:
Copy the following javascript code into your javascript console (accessible via your Browser Developer Tools), and an assistent popup will guide you trough the process :)

### Step 1: Copy the code below (hover the text, and click the copy icon on the top right)

```
let abortListener = new AbortController();

introduction();
function introduction() {
    addNode();
    let selection = null;
    selection = prompt(
        `𝙒𝙚𝙡𝙘𝙤𝙢𝙚 𝙩𝙤  📚List 2️⃣𝘾𝙡𝙞𝙥𝙗𝙤𝙖𝙧𝙙 📋
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
        } else if (selection == "automatic") {
            inputPrompt("automatic");
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
    let input=null;
    if(seperatorType=="excel" | seperatorType=="csv"| seperatorType=="automatic") {
    let inputPromptTexts = {
        automatic: "We will try to auto recognize the seperator",
        csv: "Expected CSV format eg: 1, 2, 3...",
        excel: "The values should correspond to one row or column values from excel sheet",
    };
    input = prompt("Please enter your values. "+  inputPromptTexts[seperatorType]);
    }
    else
    {
        input = prompt("Please enter your values seperated by : "+ seperatorType)
    }

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
                this.introduction();
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
            inputPrompt(seperatorType);
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
                        "You copied new content into the clipboard - Stopping List2Clipboard"
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
    document.getElementById("list2clipboard").remove();
    abortListener = new AbortController();
}

function addNode() {
    const node = document.createElement("p");
    node.id = "list2clipboard";
    node.style = 'background: black;color: white;padding: 4px 10px;border: 3px solid red'
    const textnode = document.createTextNode("Pasting from List2Clipboard. Copy new content to stop script.");
    node.appendChild(textnode);
    document.body.prepend(node);
}
```
### Step 2: Right click on the webpage where you want to paste the contents, select Inspect and go to Console tab
### Step 3: Paste the code and hit enter

A pop-up should show up with instructions to enter your data. 
