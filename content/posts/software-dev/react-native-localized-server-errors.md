---
title: 'React Native Localized Server Errors'
summary: An example of localizing server error responses and displaying them using React Native's `Alert.alert`. This implementation assumes Redux-Form
date: 2019-05-04T13:58:23+08:00
draft: false
---

An example of localizing server error responses and displaying them using React Native's `Alert.alert`. This implementation assumes Redux-Form

```javascript
import React from 'react';
import {Alert, Button} from 'react-native';
import {SubmissionError} from 'redux-form';

// localization.js
// Each type of API call must have a "default" message
const localizedStrings = {
  en: {
    somethingWentWrong: "Oops, we're sorry, something went wrong",
    'serverErrors.widgetSubmit.default':
      'Sorry your widget could not be submitted',
    'serverErrors.widgetSubmit.messageKeyA': 'The error in english',
    'serverErrors.widgetSubmit.messageKeyB': 'Another error in english',
    'serverErrors.login.default': 'Please try again',
    'serverErrors.login.messageKeyA': 'The error in english',
    'serverErrors.login.messageKeyB': 'Another error in english',
  },
};

// utils/localizeServerError.js
// key indicates where to find the errors within the localization object.
export const localizeServerError = (error, key) => {
  // handle unexpected errors
  const defaultMessage = localizedStrings[`serverErrors.${key}.default`];
  if (!error.response) return defaultMessage;

  // handle both json and text errors from server
  const messageKey =
    typeof error.response.data === 'string'
      ? error.response.data
      : error.response.data.messageKey;

  return (
    localizedStrings[`serverErrors.${key}.${messageKey}`] || defaultMessage
  );
};

// utils/alertError.js
// timeout avoids iOS crash when a modal is navigated away from while an Alert
// is being popped open
export const alertError = async (subject, message) => {
  await setTimeout(() => {
    Alert.alert(subject, message);
  }, 500);
};

// formComponent.js
export const MyComp = (handleSubmit, submitAction, navigation) => {
  const handleFormSubmission = async (values, _, {reset}) => {
    try {
      await submitAction(values);

      navigation.navigate('SUCCESS SCREEN');
      reset();
    } catch (error) {
      const subject = localizedStrings.somethingWentWrong;
      const message = localizeServerError(error);

      navigation.navigate('ERROR SCREEN');
      alertError(subject, message);

      // signal to redux-form that the submission has failed
      throw new SubmissionError({_error: message});
    }
  };

  return (
    <Button onPress={handleSubmit(handleFormSubmission)}>Submit Form</Button>
  );
};
```

Gist: https://gist.github.com/paul-ylz/0cf47fa3fdd0893305ad029a70c8c9c0
