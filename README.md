import org.openqa.selenium.By;
import org.openqa.selenium.JavascriptExecutor;
import org.openqa.selenium.WebDriver;
import org.openqa.selenium.WebElement;
import org.openqa.selenium.chrome.ChromeDriver;
import org.openqa.selenium.support.ui.ExpectedConditions;
import org.openqa.selenium.support.ui.WebDriverWait;

import java.time.Duration;

public class FitPeoAutomation {

    public static void main(String[] args) {
        // Set the path to the chromedriver executable
        System.setProperty("webdriver.chrome.driver", "/path/to/chromedriver");

        // Initialize WebDriver
        WebDriver driver = new ChromeDriver();

        try {
            // Open the FitPeo homepage
            driver.get("https://fitpeo.com");

            // Navigate to the Revenue Calculator page
            WebElement revenueCalculatorLink = driver.findElement(By.linkText("Revenue Calculator"));
            revenueCalculatorLink.click();

            // Wait until the Revenue Calculator page loads
            WebDriverWait wait = new WebDriverWait(driver, Duration.ofSeconds(10));
            wait.until(ExpectedConditions.visibilityOfElementLocated(By.id("slider")));

            // Scroll down to the slider section
            WebElement slider = driver.findElement(By.id("slider"));
            ((JavascriptExecutor) driver).executeScript("arguments[0].scrollIntoView(true);", slider);

            // Adjust the slider to set its value to 820
            setSliderValue(driver, slider, 820);

            // Update the text field to 560 and validate slider value
            WebElement textField = driver.findElement(By.id("slider-value"));
            textField.clear();
            textField.sendKeys("560");
            validateSliderValue(driver, slider, 560);

            // Select CPT codes
            selectCPTCodes(driver);

            // Validate Total Recurring Reimbursement
            validateReimbursement(driver);

        } catch (Exception e) {
            e.printStackTrace();
        } finally {
            // Close the browser
            driver.quit();
        }
    }

    private static void setSliderValue(WebDriver driver, WebElement slider, int value) {
        // Code to set the slider value to 820
        // This can be done by simulating drag-and-drop or using JavaScript
        // For simplicity, assuming a direct method to set the slider value
        ((JavascriptExecutor) driver).executeScript("arguments[0].value = arguments[1];", slider, value);
    }

    private static void validateSliderValue(WebDriver driver, WebElement slider, int expectedValue) {
        // Validate that the slider value is as expected
        String sliderValue = slider.getAttribute("value");
        if (Integer.parseInt(sliderValue) == expectedValue) {
            System.out.println("Slider value is correctly set to " + expectedValue);
        } else {
            System.out.println("Slider value mismatch. Expected: " + expectedValue + ", Actual: " + sliderValue);
        }
    }

    private static void selectCPTCodes(WebDriver driver) {
        // Select checkboxes for CPT codes
        String[] cptCodes = {"CPT-99091", "CPT-99453", "CPT-99454", "CPT-99474"};
        for (String code : cptCodes) {
            WebElement checkbox = driver.findElement(By.id(code));
            if (!checkbox.isSelected()) {
                checkbox.click();
            }
        }
    }

    private static void validateReimbursement(WebDriver driver) {
        // Validate Total Recurring Reimbursement value
        WebElement reimbursementHeader = driver.findElement(By.id("total-reimbursement"));
        String expectedReimbursement = "$110700";
        if (reimbursementHeader.getText().contains(expectedReimbursement)) {
            System.out.println("Reimbursement value is correctly displayed as " + expectedReimbursement);
        } else {
            System.out.println("Reimbursement value mismatch. Expected: " + expectedReimbursement + ", Actual: " + reimbursementHeader.getText());
        }
    }
}
