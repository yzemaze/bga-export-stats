javascript:{
/* generates CSV from user data */
function parsePlayerStats(player_page) {
    const separator = ";";
    let player = player_page.querySelector("#player_name").innerText.trim();
    let gameDivs = player_page.getElementsByClassName("palmares_game");
    for (let i = 0; i < gameDivs.length; i++) {
        let game = gameDivs[i].getElementsByClassName("gamename")[0].innerText;
        if (game == "Carcassonne") {
            var rank = "";
            let rankStr = gameDivs[i].getElementsByClassName("gamerank_no")[0];
            if (rankStr) rank = rankStr.innerText.match(/(\d+)?/)[0];
            let details = gameDivs[i].getElementsByClassName("palmares_details")[0].innerText;
            let arr = details.match(/(\d+[\s0-9]*)/g);
            var matches = Number(arr[0].replace(/\s/g, ''));
            if (arr.length == 3) {
                var won = Number(arr[1].replace(/\s/g, ''));
            } else {
                matches += Number(arr[1].replace(/\s/g, ''));
                var won = Number(arr[2].replace(/\s/g, ''));
            }
            var elo = gameDivs[i].getElementsByClassName("gamerank_value")[0].innerText;
            i = gameDivs.length;
        }
    }
    return [player, elo, rank, matches, won].join(separator);
}

/* fetches and prints group members' stats */
function exportPlayerStats() {
    let exported = document.getElementById("export_textarea");
    exported.value = "Loading...";
    let loading = async () => {
        /* for every player parse stats and concatenate outputs */
        let exported_str = "";
        const parser = new DOMParser();
        const members = document.getElementById("member_list").value.trim().split("\n");
        for (let i = 0; i < members.length; ++i) {
            const response = await fetch('https://boardgamearena.com/player?name=' + members[i]);
            const html_str = await response.text();
            const doc = parser.parseFromString(html_str, "text/html");
            try {
                exported_str += parsePlayerStats(doc) + "\n";
            } catch (err) {
                exported_str += members[i].split(/[;\t]+/)[0] + ";Account deleted;;;\n";
                console.log(members[i] + "\n" + err);
                console.log(doc);
            }
            exported.value = "Loading... " + (i+1) + "/" + members.length;
        }
        exported.value = exported_str.trim();
    };
    loading();
}

function displayExportSection() {
    let rootdiv = document.createElement("div");
    rootdiv.className = "pageheader";

    let membersdiv = document.createElement("div");
    let exportdiv = document.createElement("div");
    membersdiv.setAttribute("style", "display: inline-block; margin-right: 10px;");
    exportdiv.setAttribute("style", "display: inline-block; vertical-align: top;");
    rootdiv.appendChild(membersdiv);
    rootdiv.appendChild(exportdiv);

    let membersheader = document.createElement("h3");
    membersheader.innerText = "Names";

    let memberlist = document.createElement("textarea");
    memberlist.setAttribute("id", "member_list");
    memberlist.setAttribute("cols", "50");
    memberlist.setAttribute("rows", "10");
    if (m[1] == "player") {
        memberlist.value = document.getElementById("player_name").innerText.trim();
    }
    else {
        const members = document.querySelectorAll(".list_of_players .player_in_list a.playername");
        members.forEach(member => {
            memberlist.value += member.innerText.trim() + "\n";
        });
    }

    let startBtn = document.createElement("button");
    startBtn.setAttribute("type", "button");
    startBtn.setAttribute("id", "startBtn");
    startBtn.setAttribute("class", "bgabutton bgabutton_blue");
    startBtn.setAttribute("onclick", "exportPlayerStats()");
    startBtn.setAttribute("style", "display: block;");
    startBtn.innerText = "Start";

    membersdiv.appendChild(membersheader);
    membersdiv.appendChild(memberlist);
    membersdiv.appendChild(startBtn);

    let exportheader = document.createElement("h3");
    exportheader.innerText = "Name;Elo-Rating;Rank;Matches;Wins";

    let output = document.createElement("textarea");
    output.setAttribute("id", "export_textarea");
    output.setAttribute("cols", "50");
    output.setAttribute("rows", "10");
    output.value = "Press Start button to start the data collection.";

    exportdiv.appendChild(exportheader);
    exportdiv.appendChild(output);

    document.querySelector("#pageheader").parentNode.prepend(rootdiv);
}

m = window.location.href.match(".*boardgamearena.com/(group|player)\\?(id|name)=(\\d+|[^\&]+)");
if (m || confirm("It seems, you are not on a BGA group or player page.\nWould you still like to run the script?")) {
    try {
        displayExportSection();
    }
    catch(error) {
        alert("An error occurred:\n" + error);
    }
}
};void(0);
