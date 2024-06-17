public boolean yourMethod() {
    try {
        // Existing code
    } catch (FileNotFoundException e) {
        log.error("FileNotFound Exception Occurred");
        return false;
    } catch (IOException e) {
        log.error("IOException Occurred");
        return false;
    }

    return true;
}
