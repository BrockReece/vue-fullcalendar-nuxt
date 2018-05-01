<template>
  <div id='wrap'>
    <div id='external-events'>
      <div id='external-events-listing'>
        <h4>Draggable Events</h4>
        <div class='fc-event' v-fcevent>My Event 1</div>
        <div class='fc-event' v-fcevent>My Event 2</div>
      </div>
      <p>
        <input type='checkbox' id='drop-remove' checked='checked' />
        <label for='drop-remove'>remove after drop</label>
      </p>
    </div>

    <full-calendar :config="config"></full-calendar>
    <div style='clear:both'></div>
  </div>
</template>

<script>
  import moment from "moment";

export default {
  directives: {
    fcevent: {
      bind: function(el) {
        $(el).data("event", {
          title: $.trim($(this).text()), // use the element's text as the event title
          stick: true // maintain when user navigates (see docs on the renderEvent method)
        });

        // Make the event draggable using jQuery UI
        $(el).draggable({
          start: function(event, ui) {
            jQuery.event.dispatch.apply($(document)[0], [event]);
          },
          zIndex: 999,
          revert: true, // will cause the event to go back to its
          revertDuration: 0 // original position after the drag
        });
      }
    }
  },
  data() {
    return {
      config: {
        header: {
          left: "prev,next today",
          center: "title",
          right: "month,agendaWeek,agendaDay"
        },
        editable: true,
        droppable: true, // this allows things to be dropped onto the calendar
        dragRevertDuration: 0,
        drop: function() {
          // is the "remove after drop" checkbox checked?
          if ($("#drop-remove").is(":checked")) {
            // if so, remove the element from the "Draggable Events" list
            $(this).remove();
          }
        },
        eventDragStop: function(event, jsEvent, ui, view) {
          if (isEventOverDiv(jsEvent.clientX, jsEvent.clientY)) {
            $("#calendar").fullCalendar("removeEvents", event._id);
            var el = $("<div class='fc-event'>")
              .appendTo("#external-events-listing")
              .text(event.title);
            el.draggable({
              zIndex: 999,
              revert: true,
              revertDuration: 0
            });
            el.data("event", { title: event.title, id: event.id, stick: true });
          }
        }
      }
    };
  }
};
</script>

<style>
body {
  margin-top: 40px;
  text-align: center;
  font-size: 14px;
  font-family: "Lucida Grande", Helvetica, Arial, Verdana, sans-serif;
}

#wrap {
  width: 1100px;
  margin: 0 auto;
}

#external-events {
  float: left;
  width: 150px;
  padding: 0 10px;
  border: 1px solid #ccc;
  background: #eee;
  text-align: left;
}

#external-events h4 {
  font-size: 16px;
  margin-top: 0;
  padding-top: 1em;
}

#external-events .fc-event {
  margin: 10px 0;
  cursor: pointer;
}

#external-events p {
  margin: 1.5em 0;
  font-size: 11px;
  color: #666;
}

#external-events p input {
  margin: 0;
  vertical-align: middle;
}

#calendar {
  float: right;
  width: 900px;
}
</style>
