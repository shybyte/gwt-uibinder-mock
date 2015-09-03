# A GWT UIBinder Mock Library #

Why writing all this MVP boilerplate when you can just use UIBinder to bind the UI to your code and use this library to write easily concise tests running as fast JRE junit tests.

Lets assume you have an uibinded widget:
```
public class SterniCalculator extends Composite{  
        ...
    
        @UiField
        RadioButton maleRadioButton;
        @UiField
        RadioButton femaleRadioButton;
      
        @UiField
        Label bloodAlcoholConcentrationLabel;

        @UiHandler( { "maleRadioButton", "femaleRadioButton" })
        void onChangeGender(ValueChangeEvent<Boolean> e) {
                model.setGender(maleRadioButton.getValue() ? Gender.MALE : Gender.FEMALE);
                bloodAlcoholConcentrationLabel.setText(model.getRoundedBac() + " ‰");

        }

        ...
}
```

Lets test it:

```

@Before
public void setUp() throws Exception {
        MockitoGWTMockUtilities.disarm();
        model = new SterniCalculatorModel(2, 80, Gender.FEMALE);
}


@Test
public void changingGenderChangesBloodAlcoholConcentration() {
	widget = new SterniCalculator(model);
        reset(widget.bloodAlcoholConcentration); // reset the mockito mock
	
	widget.femaleRadioButton.setValue(true, true);

        // verify the calls to the mocked widget
	verify(widget.bloodAlcoholConcentrationLabel).setText("0.9 ‰");
      
        // or check the current value
	assertEquals("0.9 ‰", widget.bloodAlcoholConcentrationLabel.getText());
}
```