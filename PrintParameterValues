import burp.*;

import java.io.PrintWriter;
import java.util.List;

public class BurpExtender implements IBurpExtender, IProxyListener {

    private PrintWriter stdout;
    private IExtensionHelpers helpers;

    @Override
    public void registerExtenderCallbacks(IBurpExtenderCallbacks callbacks) {
        helpers = callbacks.getHelpers();
        callbacks.setExtensionName("Parameter Extractor");

        // Initialize PrintWriter for printing output to Burp Suite's UI
        stdout = new PrintWriter(callbacks.getStdout(), true);

        // Register as a Proxy listener to intercept requests
        callbacks.registerProxyListener(this);
    }

    @Override
    public void processProxyMessage(boolean messageIsRequest, IInterceptedProxyMessage message) {
        // Check if the message is a request
        if (messageIsRequest) {
            IRequestInfo requestInfo = helpers.analyzeRequest(message.getMessageInfo().getRequest());

            // Get the parameters from the request
            IParameter parameter = getParameterByName(requestInfo.getParameters(), "menu");

            if (parameter != null) {
                // Get and print the value of the parameter
                String parameterValue = parameter.getValue();
                System.out.println("Parameter Value: " + parameterValue);

                // Print the parameter value to Burp Suite's Output tab
                stdout.println("Parameter Value: " + parameterValue);
            }
        }
    }

    // Helper method to get a parameter by name
    private IParameter getParameterByName(List<IParameter> parameters, String name) {
        for (IParameter parameter : parameters) {
            if (parameter.getName().equals(name)) {
                return parameter;
            }
        }
        return null;
    }
}
