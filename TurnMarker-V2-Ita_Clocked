// Github:   https://github.com/shdwjk/Roll20API/blob/master/TurnMarker1/TurnMarker1.js
// By:       The Aaron, Arcane Scriptomancer
// Contact:  https://app.roll20.net/users/104025/the-aaron
// Modify:   HT (2021-03-31)  added timer

/*  ############################################################### */
/*  TimeMaker */
/*  ############################################################### */
var seconds = 0;
var firstq = 0;
var half = 0;
var lastq = 0;
var interval = null;

var TurnTimer = TurnTimer || (function() {
    'use strict';

    var version = "0.1.1";
});

function countDown() {
    'use strict';

    seconds = seconds - 1;

    if (seconds == firstq) {
        sendChat("Countdown Marker", "<div style='height:32px; border:1px solid #CCC; \
        font-weight: bold; background-color: white;'><img src=\
        'https://s3.amazonaws.com/files.d20.io/images/212112874/D9lRTT1JJO9E-6uyAnO5Dw/thumb.png?1617115002' \
        width='32px' height='32px' style='float: left;'> \
        <div style='float: left; height: 16px; \
        vertical-align:middle; margin: 8px 0 0 10px;'>\
        "+firstq  +" secondi residui</div></div>");
    }

    if (seconds == half) {
        sendChat("Countdown Marker", "<div style='height:32px; border:1px solid #CCC; \
        font-weight: bold; background-color: white;'><img src=\
        'https://s3.amazonaws.com/files.d20.io/images/212112873/D6JbS82lGEV1xs67ipk9Qw/thumb.png?1617115002' \
        width='32px' height='32px' style='float: left;'> \
        <div style='float: left; height: 16px; \
        vertical-align:middle; margin: 8px 0 0 10px;'>\
        "+half  +" secondi residui</div></div>");
    }

    if (seconds == lastq) {
        sendChat("Countdown Marker", "<div style='height:32px; border:1px solid #CCC; \
        font-weight: bold; background-color: white;'><img src=\
        'https://s3.amazonaws.com/files.d20.io/images/212112875/JW8DUor0FUDA09LHyz8dJA/thumb.png?1617115002' \
        width='32px' height='32px' style='float: left;'> \
        <div style='float: left; height: 16px; \
        vertical-align:middle; margin: 8px 0 0 10px;'>\
        "+lastq  +" secondi residui</div></div>");
    }

    if (seconds <= 0) {
        sendChat("Countdown Marker", "<div style='height:32px; border:1px solid #CCC; \
        font-weight: bold; background-color: white;'><img src=\
        'https://s3.amazonaws.com/files.d20.io/images/212112876/R9JzEk2akkbeWJh8nF_rpQ/thumb.png?1617115002' \
        width='32px' height='32px' style='float: left;'> \
        <div style='float: left; height: 16px; \
        vertical-align:middle; margin: 8px 0 0 10px;'>\
        Tempo Scaduto!</div></div>");

        clearInterval(interval);
    }
}

on("chat:message", function(msg) {
    'use strict'

    // When the GM types !t <number>, start a timer for <number> seconds
    if(msg.type == "api" && msg.content.indexOf("!timer ") !== -1) {
        // Clear the previous timer if running.
        clearInterval(interval);

        //log("Detected Typing");
        // Extracts the number of seconds in the command
        seconds = Number(msg.content.replace("!timer ", ""));
        //log(seconds);
		firstq = parseInt((seconds/4)*3); 
		half = parseInt((seconds/2)); 
		lastq = parseInt((seconds/4)); 

        // Begin the countdown
        interval = setInterval(countDown, 1000, seconds);

        // Inform the players that the timer has started
               sendChat("Countdown Marker", "<div style='height:32px; border:1px solid #CCC; \
            font-weight: bold; background-color: white;'><img src=\
            'https://s3.amazonaws.com/files.d20.io/images/212112877/xAuZ6ZZ-dbIDwVSUvGSoWQ/thumb.png?1617115002' \
            width='32px' height='32px' style='float: left;'> \
            <div style='float: left; height: 16px; \
            vertical-align:middle; margin: 8px 0 0 10px;'>\
            Countdown Iniziato: "+seconds+" secondi</div></div>");
    }
    // When the GM types !t, start a timer for a default number of seconds
    else if(msg.type == "api" && msg.content.indexOf("!timer") !== -1) {
        clearInterval(interval);    // Clear the previous timer if running.
        seconds = 60;               // Sets the default number of seconds
		firstq = 45; 
		half = 30;
		lastq = 15;
        // Begins the countdown
        interval = setInterval(countDown, 1000, seconds);
        sendChat("Countdown Marker", "<div style='height:32px; border:1px solid #CCC; \
            font-weight: bold; background-color: white;'><img src=\
            'https://s3.amazonaws.com/files.d20.io/images/212112877/xAuZ6ZZ-dbIDwVSUvGSoWQ/thumb.png?1617115002' \
            width='32px' height='32px' style='float: left;'> \
            <div style='float: left; height: 16px; \
            vertical-align:middle; margin: 8px 0 0 10px;'>\
            Countdown Iniziato: "+seconds+" secondi</div></div>");
    }
});

on("ready", function() {
    'use strict';

});

/* global GroupInitiative:false Mark:false */
/*  ############################################################### */
/*  TurnMarker */
/*  ############################################################### */
var TurnMarker = TurnMarker || (function(){
    "use strict";
    
    var version = '1.6.13',
        lastUpdate = 1615111164,
        schemaVersion = 1.94,
        active = false,
        threadSync = 1,
        autoPullOptions = {
            'none' : 'None',
            'npcs' : 'NPCs',
            'all'  : 'All'
        },

    getGMPlayers = (pageid) => findObjs({type:'player'})
        .filter((p)=>playerIsGM(p.id))
        .filter((p)=>undefined === pageid || p.get('lastpage') === pageid)
        .map(p=>p.id)
    ,

    sendGMPing = (left, top, pageid, playerid=null, moveAll=false) => {
        let players = getGMPlayers(pageid);
        if(players.length){
            sendPing(left,top,pageid,playerid,moveAll,players);
        }
    },

    checkInstall = function() {    
        log('-=> TurnMarker v'+version+' <=-  ['+(new Date(lastUpdate*1000))+']');

        if( ! state.hasOwnProperty('TurnMarker') || state.TurnMarker.version !== schemaVersion) {
            log('  > Updating Schema to v'+schemaVersion+' <');
            switch(state.TurnMarker && state.TurnMarker.version) {
                case 1.16:
                    state.TurnMarker.autoPull = 'none';
                    /* falls through */

                case 'UpdateSchemaVersion':
                    state.TurnMarker.version = schemaVersion;
                    break;

                default:
                    state.TurnMarker = {
                        version: schemaVersion,
                        announceRounds: true,
                        announceTurnChange: true,
                        announcePlayerInTurnAnnounce: true,
                        announcePlayerInTurnAnnounceSize: '100%',
                        autoPull: 'none',
						timeractive: true,
                        timerduration: 90,
                        autoskipHidden: true,
                        tokenName: 'Round',
                        tokenURL: 'https://s3.amazonaws.com/files.d20.io/images/4095816/086YSl3v0Kz3SlDAu245Vg/thumb.png?1400535580',
                        playAnimations: false,
                        rotation: false,
                        animationSpeed: 5,
                        scale: 1.7,
                        aura1: {
                            pulse: false,
                            size: 5,
                            color: '#ff00ff'
                        },
                        aura2: {
                            pulse: false,
                            size: 5,
                            color: '#00ff00'
                        },
                    };
                    break;
            }
        }
        if(Campaign().get('turnorder') ==='') {
            Campaign().set('turnorder','[]');
        }
        if('undefined' !== typeof GroupInitiative && GroupInitiative.ObserveTurnOrderChange){
            GroupInitiative.ObserveTurnOrderChange(handleExternalTurnOrderChange);
        }
    },

    showHelp = function(who) {
        var marker = getMarker();
        var rounds =parseInt(marker.get('bar2_value'),10);
        sendChat('',
            '/w "'+who+'" '+
'<div style="border: 1px solid black; background-color: white; padding: 3px 3px;">'+
    '<div style="font-weight: bold; border-bottom: 1px solid black;font-size: 130%;">'+
        'TurnMarker v'+version+
    '</div>'+
    '<b>Comandi</b>'+
    '<div style="padding-left:10px;"><b><span style="font-family: serif;">!tm</span></b>'+
        '<div style="padding-left: 10px;padding-right:20px">'+
            'I seguenti **arguments** possono essere passati alla funzione !tm per modificare la sua configurazione. Tutte le modifiche sono persistenti e non si modificano dopo un restart della sandbox.'+
            '<ul>'+
                    '<div style="float:right;width:40px;border:1px solid black;background-color:#ffc;text-align:center;"><span style="color: blue; font-weight:bold; padding: 0px 4px;">'+rounds+'</span></div>'+
                '<li style="border-top: 1px solid #ccc;border-bottom: 1px solid #ccc;"><b><span style="font-family: serif;">reset &lbrack;#&rbrack;</span></b> -- Imposta il **round counter** di nuovo a 0 o ad un altro valore numerico.</li> '+
                    '<div style="float:right;width:40px;border:1px solid black;background-color:#ffc;text-align:center;"><span style="color: blue; font-weight:bold; padding: 0px 4px;">'+autoPullOptions[state.TurnMarker.autoPull]+'</span></div>'+
                '<li style="border-top: 1px solid #ccc;border-bottom: 1px solid #ccc;"><b><span style="font-family: serif;">autopull &lt;mode&gt;</span></b> -- Imposta auto pulling sul token con il turno attivo.  Stato: '+_.keys(autoPullOptions)+'</li> '+
                    '<div style="float:right;width:40px;border:1px solid black;background-color:#ffc;text-align:center;">'+( state.TurnMarker.announceRounds ? '<span style="color: red; font-weight:bold; padding: 0px 4px;">ON</span>' : '<span style="color: #999999; font-weight:bold; padding: 0px 4px;">OFF</span>' )+'</div>'+
                '<li style="border-bottom: 1px solid #ccc;"><b><span style="font-family: serif;">toggle-announce</span></b> -- Quando è ad ON, ogni round viene annunciato nella chat.</li>'+
                    '<div style="float:right;width:40px;border:1px solid black;background-color:#ffc;text-align:center;">'+( state.TurnMarker.announceTurnChange ? '<span style="color: red; font-weight:bold; padding: 0px 4px;">ON</span>' : '<span style="color: #999999; font-weight:bold; padding: 0px 4px;">OFF</span>' )+'</div>'+
                '<li style="border-bottom: 1px solid #ccc;"><b><span style="font-family: serif;">toggle-announce-turn</span></b> -- Quando è ad ON, la transizione tra i turni visibili viene annunciata in chat.</li> '+
                    '<div style="float:right;width:40px;border:1px solid black;background-color:#ffc;text-align:center;">'+( state.TurnMarker.announcePlayerInTurnAnnounce ? '<span style="color: red; font-weight:bold; padding: 0px 4px;">ON</span>' : '<span style="color: #999999; font-weight:bold; padding: 0px 4px;">OFF</span>' )+'</div>'+
                '<li style="border-bottom: 1px solid #ccc;"><b><span style="font-family: serif;">toggle-announce-player</span></b> -- Quando è ad ON, il nome del giocatore, o dei giocatori che controllano il personaggio del turno corrente sono inclusi nel annuncio in chat.</li> '+
                    '<div style="float:right;width:40px;border:1px solid black;background-color:#ffc;text-align:center;">'+( state.TurnMarker.autoskipHidden ? '<span style="color: red; font-weight:bold; padding: 0px 4px;">ON</span>' : '<span style="color: #999999; font-weight:bold; padding: 0px 4px;">OFF</span>' )+'</div>'+
                '<li style="border-bottom: 1px solid #ccc;"><b><span style="font-family: serif;">toggle-skip-hidden</span></b> -- quando è ad ON, ordine dei turni avanza automaticamente senza considerare i turni dei token nascosti.</li> '+
                    '<div style="float:right;width:40px;border:1px solid black;background-color:#ffc;text-align:center;">'+( state.TurnMarker.playAnimations ? '<span style="color: red; font-weight:bold; padding: 0px 4px;">ON</span>' : '<span style="color: #999999; font-weight:bold; padding: 0px 4px;">OFF</span>' )+'</div>'+
                '<li style="border-bottom: 1px solid #ccc;"><b><span style="font-family: serif;">toggle-animations</span></b> -- Attiva le animazioni del marker di turno. [Experimental!]</li> '+
                    '<div style="float:right;width:40px;border:1px solid black;background-color:#ffc;text-align:center;">'+( state.TurnMarker.rotation ? '<span style="color: red; font-weight:bold; padding: 0px 4px;">ON</span>' : '<span style="color: #999999; font-weight:bold; padding: 0px 4px;">OFF</span>' )+'</div>'+
                '<li style="border-bottom: 1px solid #ccc;"><b><span style="font-family: serif;">toggle-rotate</span></b> -- Quando è ad ON, il marker dei turni ruota lentamente in senso orario. [Animazione]</li> '+
                    '<div style="float:right;width:40px;border:1px solid black;background-color:#ffc;text-align:center;">'+( state.TurnMarker.aura1.pulse ? '<span style="color: red; font-weight:bold; padding: 0px 4px;">ON</span>' : '<span style="color: #999999; font-weight:bold; padding: 0px 4px;">OFF</span>' )+'</div>'+
                '<li style="border-bottom: 1px solid #ccc;"><b><span style="font-family: serif;">toggle-aura-1</span></b> -- Quando è ad ON, aura 2 pulsa in modo intermtittente. [Animazione]</li> '+
                    '<div style="float:right;width:40px;border:1px solid black;background-color:#ffc;text-align:center;">'+( state.TurnMarker.aura2.pulse ? '<span style="color: red; font-weight:bold; padding: 0px 4px;">ON</span>' : '<span style="color: #999999; font-weight:bold; padding: 0px 4px;">OFF</span>' )+'</div>'+
                '<li style="border-bottom: 1px solid #ccc;"><b><span style="font-family: serif;">toggle-aura-2</span></b> -- Quando è ad ON, aura 2 pulsa in modo intermittente. [Animazione]</li> '+
				 '<div style="float:right;width:40px;border:1px solid black;background-color:#ffc;text-align:center;">'+( state.TurnMarker.timeractive ? '<span style="color: red; font-weight:bold; padding: 0px 4px;">ON</span>' : '<span style="color: #999999; font-weight:bold; padding: 0px 4px;">OFF</span>' )+'</div>'+
                '<li style="border-bottom: 1px solid #ccc;"><b><span style="font-family: serif;">turn-timer </span></b> -- Ativa o disattiva il **timer** di countdown nel turno.</li>'+
				'<div style="float:right;width:40px;border:1px solid black;background-color:#ffc;text-align:center;"><span style="color: blue; font-weight:bold; padding: 0px 4px;">'+(state.TurnMarker.timerduration)+'</span></div>'+
                '<li style="border-top: 1px solid #ccc;border-bottom: 1px solid #ccc;"><b><span style="font-family: serif;">timer-duration &lbrack;#&rbrack;</span></b> -- Imposta la durata, in secondi, del turno di ogni giocatore .</li> '+
				'</li> '+
            '</ul>'+
        '</div>'+
    '</div>'+
    '<div style="padding-left:10px;"><b><span style="font-family: serif;">!eot</span></b>'+
        '<div style="padding-left: 10px;padding-right:20px;">'+
            'I giocatori possono eseguire questo comando per avanzare alprossimo turno in ordine di iniziativa.  Solo se il giocatore controlla un tiken attivo nel turno o se viene esguito dal GM questo comando funziona correttamente.'+
        '</div>'+
    '</div>'+
'</div>', 
            );
    },

    handleInput = function(msg){
        var who, tokenized, command;

        if (msg.type !== "api") {
            return;
        }

        who=(getObj('player',msg.playerid)||{get:()=>'API'}).get('_displayname');
        tokenized = msg.content.split(/\s+/);
        command = tokenized[0];

        switch(command) {
            case "!tm":
            case "!turnmarker": {
                    if(!playerIsGM(msg.playerid)){
                        return;
                    }
                    let tokens=_.rest(tokenized),marker,value;
                    switch (tokens[0]) {
                        case 'reset':
                            marker = getMarker();
                            value = parseInt(tokens[1],10)||0;
                            marker.set({
                                name: state.TurnMarker.tokenName+' '+value,
                                bar2_value: value
                            });
                            sendChat('','/w "'+who+'" <b>Round</b> counter resettato a <b>'+value+'</b>.');
                            break;

                        case 'ping-target':
                            var obj=getObj('graphic',tokens[1]);
                            if(obj){
                                sendGMPing(obj.get('left'),obj.get('top'),obj.get('pageid'),null,true);
                            }
                            break;

                        case 'autopull':
                            if(_.contains(_.keys(autoPullOptions), tokens[1])){
                                state.TurnMarker.autoPull=tokens[1];
                                sendChat('','/w "'+who+'" <b>AutoPull</b> è stato impostato a <b>'+(autoPullOptions[state.TurnMarker.autoPull])+'</b>.');
                            } else {
                                sendChat('','/w "'+who+'" "'+tokens[1]+'" non è una opzione valida per <b>AutoPull</b>.  Specifica uno dei seguenti: '+_.keys(autoPullOptions).join(', ')+'</b>.');
                            }
                            break;

                        case 'toggle-announce':
                            state.TurnMarker.announceRounds=!state.TurnMarker.announceRounds;
                            sendChat('','/w "'+who+'" <b>Announce Rounds</b>iè in stato <b>'+(state.TurnMarker.announceRounds ? 'ON':'OFF' )+'</b>.');
                            break;
						
						case 'turn-timer':
					        state.TurnMarker.timeractive=!state.TurnMarker.timeractive;
                            sendChat('','/w "'+who+'" Il <b>Timer </b> è in stato <b>'+(state.TurnMarker.timeractive ? 'ON':'OFF' )+'</b>.');
                            break;
							
						 case 'timer-duration':
						  if(Number(tokens[1]) >0){
                               state.TurnMarker.timerduration=tokens[1];
                               sendChat('','/w "'+who+'" La durata del <b>Timer </b> è ora impostata  <b>'+tokens[1]+'</b> secondi.');
                            } 
							else {  
								sendChat('','/w "'+who+'" Il valore della <b>Durata del Timer </b> non può essere  <b>0</b>. Inserire un valore Corretto.');
								  }
								
                            break;

                        case 'toggle-announce-turn':
                            state.TurnMarker.announceTurnChange=!state.TurnMarker.announceTurnChange;
                            sendChat('','/w "'+who+'" <b>Announce Cambio Turno</b> è in stato <b>'+(state.TurnMarker.announceTurnChange ? 'ON':'OFF' )+'</b>.');
                            break;

                        case 'toggle-announce-player':
                            state.TurnMarker.announcePlayerInTurnAnnounce=!state.TurnMarker.announcePlayerInTurnAnnounce;
                            sendChat('','/w "'+who+'" <b>Nome Giocatore nel Announce</b> è in stato <b>'+(state.TurnMarker.announcePlayerInTurnAnnounce ? 'ON':'OFF' )+'</b>.');
                            break;

                        case 'toggle-skip-hidden':
                            state.TurnMarker.autoskipHidden=!state.TurnMarker.autoskipHidden;
                            sendChat('','/w "'+who+'" <b>Auto-skip Nascoto</b> è in stato <b>'+(state.TurnMarker.autoskipHidden ? 'ON':'OFF' )+'</b>.');
                            break;

                        case 'toggle-animations':
                            state.TurnMarker.playAnimations=!state.TurnMarker.playAnimations;
                            if(state.TurnMarker.playAnimations) {
                                stepAnimation(threadSync);
                            } else {
                                marker = getMarker();
                                marker.set({
                                    aura1_radius: '',
                                    aura2_radius: ''
                                });
                            }

                            sendChat('','/w "'+who+'" <b>Le animazioni</b> sono in stato <b>'+(state.TurnMarker.playAnimations ? 'ON':'OFF' )+'</b>.');
                            break;

                        case 'toggle-rotate':
                            state.TurnMarker.rotation=!state.TurnMarker.rotation;
                            sendChat('','/w "'+who+'" <b>Rotazione</b> è in stato <b>'+(state.TurnMarker.rotation ? 'ON':'OFF' )+'</b>.');
                            break;

                        case 'toggle-aura-1':
                            state.TurnMarker.aura1.pulse=!state.TurnMarker.aura1.pulse;
                            sendChat('','/w "'+who+'" <b>Aura 1</b> è in stato <b>'+(state.TurnMarker.aura1.pulse ? 'ON':'OFF' )+'</b>.');
                            break;

                        case 'toggle-aura-2':
                            state.TurnMarker.aura2.pulse=!state.TurnMarker.aura2.pulse;
                            sendChat('','/w "'+who+'" <b>Aura 2</b> è in stato <b>'+(state.TurnMarker.aura2.pulse ? 'ON':'OFF' )+'</b>.');
                            break;

                        default:
                        case 'help':
                            showHelp(who);
                            break;

                    }
                }
                break;

            case "!eot":
                requestTurnAdvancement(msg.playerid);   
                break;
        }
    },

    getMarker = function(){  
        var marker = findObjs({
            imgsrc: state.TurnMarker.tokenURL,
            pageid: Campaign().get("playerpageid")    
        })[0];

        if (marker === undefined) {
            marker = createObj('graphic', {
                name: state.TurnMarker.tokenName+' 0',
                pageid: Campaign().get("playerpageid"),
                layer: 'gmlayer',
                imgsrc: state.TurnMarker.tokenURL,
                left: 0,
                top: 0,
                height: 70,
                width: 70,
                bar2_value: 0,
                showplayers_name: true,
                showplayers_aura1: true,
                showplayers_aura2: true
            });
        }
        if(!TurnOrder.HasTurn(marker.id)) {
            TurnOrder.AddTurn({
                id: marker.id,
                pr: -1,
                custom: "",
                pageid: marker.get('pageid')
            });
        }
        return marker;    
    },

    stepAnimation = function( sync ){
        if (!state.TurnMarker.playAnimations || sync !== threadSync) {
            return;
        }
        var marker=getMarker();
        if(active === true) {
            var rotation=(marker.get('bar1_value')+state.TurnMarker.animationSpeed)%360;
            marker.set('bar1_value', rotation );
            if(state.TurnMarker.rotation) {
                marker.set( 'rotation', rotation );
            }
            if( state.TurnMarker.aura1.pulse ) {
                marker.set('aura1_radius', Math.abs(Math.sin(rotation * (Math.PI/180))) * state.TurnMarker.aura1.size );
            } else {
                marker.set('aura1_radius','');
            }
            if( state.TurnMarker.aura2.pulse  ) {
                marker.set('aura2_radius', Math.abs(Math.cos(rotation * (Math.PI/180))) * state.TurnMarker.aura2.size );
            } else {
                marker.set('aura2_radius','');
            }
            setTimeout(_.bind(stepAnimation,this,sync), 100);
        }
    },
    checkForTokenMove = function(obj){
        var turnOrder, current, marker;
        if(active) {
            turnOrder = TurnOrder.Get();
            current = _.first(turnOrder);
            if( obj && current && current.id === obj.id) {
               threadSync++;
                
                marker = getMarker();
                marker.set({
                    "layer": obj.get("layer"),
                    "top": obj.get("top"),
                    "left": obj.get("left")
                });
                
               setTimeout(_.bind(stepAnimation,this,threadSync), 300);
            }
        }
    },
    requestTurnAdvancement = function(playerid){
        if(active) {
            let turnOrder = TurnOrder.Get(),
                current = getObj('graphic',_.first(turnOrder).id),
                character = getObj('character',(current && current.get('represents')));
            if(playerIsGM(playerid) ||
                ( current &&
                       ( _.contains(current.get('controlledby').split(','),playerid) ||
                       _.contains(current.get('controlledby').split(','),'all') )
                    ) ||
                ( character &&
                       ( _.contains(character.get('controlledby').split(','),playerid) ||
                       _.contains(character.get('controlledby').split(','),'all') )
                    )
                )
            {
                TurnOrder.Next();
                turnOrderChange(true);
            }
        }
    },
    announceRound = function(round){
        if(state.TurnMarker.announceRounds) {
            sendChat(
                'End of the Turn', 
                "/direct "+
                "<div style='"+
//				"<div style=' margin: 1em 0 1em 0;'>"  +                
                    'display: block;'+
					'border-radius: .3em;'+
					'background-color: #4B0082;'+
                    'border: 3px solid #808080;'+
                    'font-size: 20px;'+
                    'text-align:center;'+
                    'vertical-align: top;'+
                    'color: white;'+
                    'font-weight:bold;'+
					'margin: 1em 0 1em 0;'+
					' box-shadow: 0 0 25px 2px #999;'+
                    'padding: 5px 5px;'+
                "'>"+
                    "<img src='"+state.TurnMarker.tokenURL+"' style='width:20px; height:20px; padding: 0px 5px;' />"+
                    "Round "+ round +
                    "<img src='"+state.TurnMarker.tokenURL+"' style='width:20px; height:20px; padding: 0px 5px;' />"+
                "</div>"+
                '<a style="position:relative;z-index:10000; top:-1em; float: right;font-size: .6em; color: white; border: 1px solid #cccccc; border-radius: 1em; margin: 0 .1em; font-weight: bold; padding: .1em .4em;" href="!tm reset ?{Numero Round|0}">Reset &'+'#x21ba;</a>'
            );
        }
    },

    turnOrderChange = function(FirstTurnChanged){
        var marker = getMarker();
                    
        if( !Campaign().get('initiativepage') ) {
            return;
        }
        
        var turnOrder = TurnOrder.Get();
        
        if (!turnOrder.length) {
            return;
        }

        var current = _.first(turnOrder);

        if(state.TurnMarker.playAnimations) {
            threadSync++;
            setTimeout(_.bind(stepAnimation,this,threadSync), 300);
        }
        
        if (current.id === "-1") {
            return;
        }
      
        handleMarkerTurn();

        if(state.TurnMarker.autoskipHidden) {
            TurnOrder.NextVisible();
            handleMarkerTurn();
        }

        turnOrder=TurnOrder.Get();

        if(turnOrder[0].id === marker.id) {
            return;
        }

        current = _.first(TurnOrder.Get());
        
        var currentToken = getObj("graphic", turnOrder[0].id),
            currentChar = getObj('character', (currentToken||{get:_.noop}).get('represents'));
        if(currentToken) {

            if(FirstTurnChanged) {
                handleAnnounceTurnChange();
            }
            
            var size = Math.max(currentToken.get("height"),currentToken.get("width")) * state.TurnMarker.scale;
              
            if (marker.get("layer") === "gmlayer" && currentToken.get("layer") !== "gmlayer") {
                marker.set({
                    "top": currentToken.get("top"),
                    "left": currentToken.get("left"),
                    "height": size,
                    "width": size
                });
                setTimeout(function() {
                    marker.set({
                        "layer": currentToken.get("layer")
                    });    
                }, 500);
            } else {
                marker.set({
                    "layer": currentToken.get("layer"),
                    "top": currentToken.get("top"),
                    "left": currentToken.get("left"),
                    "height": size,
                    "width": size
                });   
            }
            toFront(currentToken);

            if( 'all' === state.TurnMarker.autoPull ||
                ('npcs' === state.TurnMarker.autoPull && (
                    '' === currentToken.get('controlledby') &&
                    ( !currentChar || '' === currentChar.get('controlledby'))
                ))
            ){
                sendGMPing(currentToken.get('left'),currentToken.get('top'),currentToken.get('pageid'),null,true);
            }
        }
    },

    handleDestroyGraphic = function(obj){
        if(TurnOrder.HasTurn(obj.id)){
            let prev=JSON.parse(JSON.stringify(Campaign()));
            TurnOrder.RemoveTurn(obj.id);
            handleTurnOrderChange(Campaign(),prev);
        }
    },

    handleTurnOrderChange = function(obj, prev) {
        var prevOrder=JSON.parse(prev.turnorder||'[]');
        var objOrder=JSON.parse(obj.get('turnorder')||'[]');

        if( _.isArray(prevOrder) &&
            _.isArray(objOrder) &&
            prevOrder.length &&
            objOrder.length &&
            objOrder[0].id !== prevOrder[0].id
          ) {
            turnOrderChange(true);
        }
    },

    handleExternalTurnOrderChange = function() {
        var marker = getMarker(),
            turnorder = Campaign().get('turnorder'),
            markerTurn;

        turnorder = ('' === turnorder) ? [] : JSON.parse(turnorder);
        markerTurn = _.filter(turnorder, function(i){
            return marker.id === i.id;
        })[0];

        if(markerTurn.pr !== -1){
            markerTurn.pr = -1;
            turnorder =_.union([markerTurn], _.reject(turnorder, function(i){
                return marker.id === i.id || (getObj('graphic',i.id)||{get:_.noop}).get('imgsrc')===state.TurnMarker.tokenURL;
            }));
            Campaign().set('turnorder',JSON.stringify(turnorder));
        }
        _.defer(dispatchInitiativePage);
    },

    handleMarkerTurn = function(){
        var marker = getMarker(),
            turnOrder = TurnOrder.Get(),
            round;

        if(turnOrder[0].id === marker.id) {
            round=parseInt(marker.get('bar2_value'))+1;
            marker.set({
                name: state.TurnMarker.tokenName+' '+round,
                bar2_value: round
            });
            announceRound(round);
            TurnOrder.Next();
        }
    },
    handleAnnounceTurnChange = function(){

        if(state.TurnMarker.announceTurnChange ) {
            var marker = getMarker();
            var turnOrder = TurnOrder.Get();
            var currentToken = getObj("graphic", turnOrder[0].id);
            if('gmlayer' === currentToken.get('layer')) {
                return;
            }
            var previousTurn=_.last(_.filter(turnOrder,function(element){
                var token=getObj("graphic", element.id);
                return token &&
                    token.get('layer') !== 'gmlayer' &&
                    element.id !== marker.id;
            }));
            
            /* find previous token. */
            var previousToken = getObj("graphic", previousTurn.id);
            var pImage=previousToken.get('imgsrc');
            var cImage=currentToken.get('imgsrc');
            var pRatio=previousToken.get('width')/previousToken.get('height');
            var cRatio=currentToken.get('width')/currentToken.get('height');
            
            var pNameString="Il turno si è concluso.";
            if(previousToken && previousToken.get('showplayers_name')) {
                pNameString='<span style=\''+
                        'font-family: Baskerville, "Baskerville Old Face", "Goudy Old Style", Garamond, "Times New Roman", serif;'+
                        'text-decoration: underline;'+
                        'font-size: 130%;'                        +
                    '\'>'+
                        previousToken.get('name')+
                    '</span>\'s turn is done.';                
            }
            
            var cNameString='Il nuovo turno è iniziato.';
            if(currentToken && currentToken.get('showplayers_name')) {
                cNameString='<span style=\''+
                    'font-family: Baskerville, "Baskerville Old Face", "Goudy Old Style", Garamond, "Times New Roman", serif;'+
                    'text-decoration: underline;'+
                    'font-size: 130%;'+
                '\'>'+
                    currentToken.get('name')+
                '</span>, it\'s now your turn!';
            }
 
            
            var PlayerAnnounceExtra='<a style="position:relative;z-index:10000; top:-1em;float: right;font-size: .6em; color: white; border: 1px solid #cccccc; border-radius: 1em; margin: 0 .1em; font-weight: bold; padding: .1em .4em;" href="!eot">EOT &'+'#x21e8;</a>';
            if(state.TurnMarker.announcePlayerInTurnAnnounce) {
                var Char=currentToken.get('represents');
                if(Char) {
                    Char=getObj('character',Char);
                    if(Char && _.isFunction(Char.get)) {
                        var Controllers=Char.get('controlledby').split(',');
                        _.each(Controllers,function(c){
                            switch(c) {
                                case 'all':
                                    PlayerAnnounceExtra+='<div style="'+
                                            'padding: 0px 5px;'+
                                            'font-weight: bold;'+
                                            'text-align: center;'+
                                            'font-size: '+state.TurnMarker.announcePlayerInTurnAnnounceSize+';'+
                                            'border: 5px solid black;'+
                                            'background-color: white;'+
                                            'color: black;'+
                                            'letter-spacing: 3px;'+
                                            'line-height: 130%;'+
                                        '">'+
                                            'All'+
                                        '</div>';
                                    break;

                                default:
                                    var player=getObj('player',c);
                                    if(player) {
                                        var PlayerColor=player.get('color');
                                        var PlayerName=player.get('displayname');
                                        PlayerAnnounceExtra+='<div style="'+
                                                'padding: 5px;'+
                                                'text-align: center;'+
                                                'font-size: '+state.TurnMarker.announcePlayerInTurnAnnounceSize+';'+
                                                'background-color: '+PlayerColor+';'+
                                                'text-shadow: '+
                                                    '-1px -1px 1px #000,'+
                                                    ' 1px -1px 1px #000,'+
                                                    '-1px  1px 1px #000,'+
                                                    ' 1px  1px 1px #000;'+
                                                'letter-spacing: 3px;'+
                                                'line-height: 130%;'+
                                            '">'+
                                                PlayerName+
                                            '</div>';
                                    }
                                    break;
                            }
                        });
                    }
                }
            }
            
            var tokenSize=70;
            if (state.TurnMarker.timeractive) 
				{
					sendChat(
                'Turn Order', 
                "/direct "+
                //"<div style='border: 3px solid #808080; background-color: #4B0082; border-radius: .3em; padding: 1em;color: white; '>"+
				"<div style='display: block; border: 3px solid #808080; border-radius: .3em; background-color: #4B0082; color: white; padding: 1px 1px; box-shadow: 0 0 25px 2px #999; margin: 1em 0 1em 0;'>"  +                
				  '<div style="text-align: left;  margin: 5px 5px;">'+
                        '<a style="position:relative;z-index:1000;float:left; background-color:transparent;border:0;padding:0;margin:0;display:block;" href="!tm ping-target '+previousToken.id+'">'+
                            "<img src='"+pImage+"' style='width:"+Math.round(tokenSize*pRatio)+"px; height:"+tokenSize+"px; padding: 0px 2px;' />"+
                        '</a>'+
                         pNameString+
                    '</div>'+
                    '<div style="text-align: right; margin: 5px 5px; position: relative; vertical-align: text-bottom;">'+
                        '<a style="position:relative;z-index:1000;float:right; background-color:transparent;border:0;padding:0;margin:0;display:block;" href="!tm ping-target '+currentToken.id+'">'+
                            "<img src='"+cImage+"' style='width:"+Math.round(tokenSize*cRatio)+"px; height:"+tokenSize+"px; padding: 0px 2px;' />"+
                        '</a>'+
                         '<span style="position:absolute; bottom: 0;right:'+Math.round((tokenSize*cRatio)+6)+'px;">'+
                            cNameString+
                         '</span>'+
                        '<div style="clear:both;"></div>'+
                    '</div>'+
                     PlayerAnnounceExtra+
                    '<div style="clear:both;"></div>'+
                "</div>"
            ),
					sendChat('Timer','!timer '+state.TurnMarker.timerduration+' ');
					
				}
				else 
			{
					sendChat(
                'Turn Order', 
                "/direct "+
                //"<div style='border: 3px solid #808080; background-color: #4B0082; border-radius: .3em; padding: 1em;color: white; '>"+
				"<div style='display: block; border: 3px solid #808080; border-radius: .3em; background-color: #4B0082; color: white; padding: 1px 1px; box-shadow: 0 0 25px 2px #999; margin: 1em 0 1em 0;'>"  +                
				  '<div style="text-align: left;  margin: 5px 5px;">'+
                        '<a style="position:relative;z-index:1000;float:left; background-color:transparent;border:0;padding:0;margin:0;display:block;" href="!tm ping-target '+previousToken.id+'">'+
                            "<img src='"+pImage+"' style='width:"+Math.round(tokenSize*pRatio)+"px; height:"+tokenSize+"px; padding: 0px 2px;' />"+
                        '</a>'+
                         pNameString+
                    '</div>'+
                    '<div style="text-align: right; margin: 5px 5px; position: relative; vertical-align: text-bottom;">'+
                        '<a style="position:relative;z-index:1000;float:right; background-color:transparent;border:0;padding:0;margin:0;display:block;" href="!tm ping-target '+currentToken.id+'">'+
                            "<img src='"+cImage+"' style='width:"+Math.round(tokenSize*cRatio)+"px; height:"+tokenSize+"px; padding: 0px 2px;' />"+
                        '</a>'+
                         '<span style="position:absolute; bottom: 0;right:'+Math.round((tokenSize*cRatio)+6)+'px;">'+
                            cNameString+
                         '</span>'+
                        '<div style="clear:both;"></div>'+
                    '</div>'+
                     PlayerAnnounceExtra+
                    '<div style="clear:both;"></div>'+
                "</div>"
            )	
			};
			
			
        }
    },
    resetMarker = function() {
        active=false;
        threadSync++;

        var marker = getMarker();
        
        marker.set({
            layer: "gmlayer",
            aura1_radius: '',
            aura2_radius: '',
            left: 35,
            top: 35,
            height: 70,
            width: 70,
            rotation: 0,
            bar1_value: 0
        });
    },
    startMarker = function() {
        var marker = getMarker();

        if(state.TurnMarker.playAnimations && state.TurnMarker.aura1.pulse) {
            marker.set({
                aura1_radius: state.TurnMarker.aura1.size,
                aura1_color: state.TurnMarker.aura1.color
            });   
        }
        if(state.TurnMarker.playAnimations && state.TurnMarker.aura2.pulse) {
            marker.set({
                aura2_radius: state.TurnMarker.aura2.size,
                aura2_color: state.TurnMarker.aura2.color
            });   
        }
        active=true;
        stepAnimation(threadSync);
        turnOrderChange(true);
    },
    dispatchInitiativePage = function(){
        if( !Campaign().get('initiativepage') ) {
            resetMarker();
        } else {
            startMarker();
        }
    },
    registerEventHandlers = function(){        
        on("change:campaign:initiativepage", dispatchInitiativePage );
        on("change:campaign:turnorder", handleTurnOrderChange );
        on("change:graphic:lastmove", checkForTokenMove );
        on("destroy:graphic", handleDestroyGraphic );
        on("chat:message", handleInput );

        dispatchInitiativePage();
    }
    ;

    return {
        CheckInstall: checkInstall,
        RegisterEventHandlers: registerEventHandlers,
		TurnOrderChange: handleExternalTurnOrderChange
    };

}());

on("ready",function(){
    'use strict';

	TurnMarker.CheckInstall(); 
	TurnMarker.RegisterEventHandlers();
});

var TurnOrder = TurnOrder || (function() {
    "use strict";

    return {
        Get: function(){
            var to=Campaign().get("turnorder");
            to=(''===to ? '[]' : to); 
            return JSON.parse(to);
        },
        Set: function(turnOrder){
            Campaign().set({turnorder: JSON.stringify(turnOrder)});
        },
        Next: function(){
            this.Set(TurnOrder.Get().rotate(1));
            if("undefined" !== typeof Mark && _.has(Mark,'Reset') && _.isFunction(Mark.Reset)) {
                Mark.Reset();
            }
        },
        NextVisible: function(){
            var turns=this.Get();
            var context={skip: 0};
            var found=_.find(turns,function(element){
                var token=getObj("graphic", element.id);
                if(
                    (undefined !== token) &&
                    (token.get('layer')!=='gmlayer')
                )
                {
                    return true;
                }
                else
                {
                    this.skip++;
                }
            },context);
            if(undefined !== found && context.skip>0)
            {
                this.Set(turns.rotate(context.skip));
            }
        },
        HasTurn: function(id){
         return (_.filter(this.Get(),function(turn){
                return id === turn.id;
            }).length !== 0);
        },
        AddTurn: function(entry){
            var turnorder = this.Get();
            turnorder.push(entry);
            this.Set(turnorder);
        },
        RemoveTurn: function(id){
            this.Set(_.reject(this.Get(),(o)=>o.id===id));
        }

    };
}());

Object.defineProperty(Array.prototype, 'rotate', {
    enumerable: false,
    writable: true
});

Array.prototype.rotate = (function() {
    "use strict";
    var unshift = Array.prototype.unshift,
        splice = Array.prototype.splice;

    return function(count) {
        var len = this.length >>> 0;
            count = count >> 0;

        unshift.apply(this, splice.call(this, count % len, len));
        return this;
    };
}());
