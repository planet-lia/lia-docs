<style>
/* Style the tab */
.tab {
    overflow: hidden;
}

/* Style the buttons inside the tab */
.tab button {
    background-color: #FBFBFB;
    color: rgb(0, 0, 0, .87);
    float: left;
    margin-top: 24px;
    outline: none;
    cursor: pointer;
    padding: 6px 16px;
    transition: 0.3s;
}

/* Change background color of buttons on hover */
.tab button:hover {
    color: #34C4A2;
}

/* Create an active/current tablink class */
.tab button.active {
    color: #019170;
}

/* Style the tab content */
.tabcontent {
    display: none;
    border-top: none;
}
.tabcontent pre{
    margin:0 -24px 0
}
</style>
<body>


<script>
function changeLanguage(evt, lang, tabClass, codeClass) {
    var i, tabcontent, tablinks;
    tabcontent = document.getElementsByClassName(codeClass);
    for (i = 0; i < tabcontent.length; i++) {
        tabcontent[i].style.display = "none";
    }
    tablinks = document.getElementsByClassName(tabClass);
    for (i = 0; i < tablinks.length; i++) {
        tablinks[i].className = tablinks[i].className.replace(" active", "");
    }
    document.getElementById(lang).style.display = "block";
    evt.currentTarget.className += " active";
}
</script>