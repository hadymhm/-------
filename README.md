# -------// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

contract HorseRace {
    address public owner;
    uint public entryFee;
    uint public maxPlayers;
    bool public raceStarted;

    address[] public players;

    constructor(uint _entryFee, uint _maxPlayers) {
        owner = msg.sender;
        entryFee = _entryFee;
        maxPlayers = _maxPlayers;
    }

    modifier onlyOwner() {
        require(msg.sender == owner, "فقط المالك يستطيع تنفيذ هذا");
        _;
    }

    function joinRace() public payable {
        require(!raceStarted, "السباق قد بدأ بالفعل");
        require(msg.value == entryFee, "قيمة الدخول غير صحيحة");
        require(players.length < maxPlayers, "اكتمل عدد المشاركين");

        players.push(msg.sender);

        if (players.length == maxPlayers) {
            startRace();
        }
    }

    function startRace() internal {
        raceStarted = true;
        uint winnerIndex = uint(keccak256(abi.encodePacked(block.timestamp, block.difficulty))) % players.length;
        address winner = players[winnerIndex];

        payable(winner).transfer(address(this).balance);
        delete players;
        raceStarted = false;
    }

    function getPlayers() public view returns (address[] memory) {
        return players;
    }

    function contractBalance() public view returns (uint) {
        return address(this).balance;
    }
}
روبوت ذكي يعمل تلقائيًا لخوض سباقات الخيول في  Pegaxy مثل Web3 ألعاب  يقوم بالانضمام إلى السباقات، اختيار الخيول تلقائيا وتتبع الأرباح دون تدخل بشري.  مصمم خصيصًا لزيادة الأرباح وتقليل الوقت المهدر.
