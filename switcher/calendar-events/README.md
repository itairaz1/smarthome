# Integrate Calendar events with switcher
Integration between your google calendar events and [Switcher](https://www.home-assistant.io/integrations/switcher_kis/)
This allows you: when you have event on a specific calendar (it can be shared between your family members), it will turn your boiler when the event starts for the same event duration.

## Disclaimer
I don't have any relation to switcher, switcher integration with HA, home assistant or anything in this area.
Everything that you are doing is on your own risk.

## Prerequisites
* Switcher device
* Google account
* Home assistant
* Working integration between switcher and home assistant
* Access to home assistant file system (like Samba share)
* Basic knowledge in yaml

## Steps
1. (Optional) Add a calendar for boiler or use your main calendar. 
2. Follow the Prerequisites section in https://www.home-assistant.io/integrations/calendar.google/
3. Add the following to `configuration.yaml` and complete the credential based on the previous section:
    ```yaml
    google:
      client_id: YOUR_CLIENT_ID
      client_secret: YOUR_CLIENT_SECRET
    ```
4. Edit `google_calendars.yaml` in `config` directory (same directory where `configuration.yaml` located) and locate your calendar (you can search for the calendar name). Then add a new item in entities list like below:

    Before:
    ```yaml
    - cal_id: xxx@group.calendar.google.com
      entities:
      - device_id: home 
        ignore_availability: true
        name: Home
        track: false
    ```
    After:
    ```yaml
    - cal_id: xxx@group.calendar.google.com
      entities:
      - device_id: home
        ignore_availability: true
        name: Home
        track: false
      - device_id: boiler_event # <-- choose a name, and keep it for later
        ignore_availability: true
        name: Boiler event # <-- choose a display name 
        track: true
        search: 'boiler' # <-- the content of the event
    ```
5. Since switcher integration has a bug that doesn't allow automation, please download [switcher_kis directory](switcher_kis) and place it in `custom_components` directory
6. Copy the content of [automation.yaml](automation.yaml) to your `automation.yaml` file and replace your switcher entity id
7. Restart home assistant and test it

## Limitations
* Event's duration is support up to 90m
* Edit the event (like duration) when the event already started is not supported
