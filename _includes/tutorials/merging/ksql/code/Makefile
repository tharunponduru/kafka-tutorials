STEPS_DIR := tutorial-steps
DEV_OUTPUTS_DIR := $(STEPS_DIR)/dev/outputs
TEST_OUTPUTS_DIR := $(STEPS_DIR)/test/outputs
PROD_OUTPUTS_DIR := $(STEPS_DIR)/prod/outputs
TEMP_DIR := $(shell mktemp -d)

tutorial:
	rm -r $(DEV_OUTPUTS_DIR) || true
	rm -r $(TEST_OUTPUTS_DIR) || true
	mkdir $(DEV_OUTPUTS_DIR)
	mkdir -p $(TEST_OUTPUTS_DIR)
	harness-runner ../../../../../_data/harnesses/merging/ksql.yml $(TEMP_DIR)
	diff --strip-trailing-cr --ignore-space-change $(STEPS_DIR)/dev/expected-description.log $(DEV_OUTPUTS_DIR)/describe-stream/output-0.log
	bash -c 'diff --strip-trailing-cr <(sort $(STEPS_DIR)/dev/expected-transient.log) <(sort $(DEV_OUTPUTS_DIR)/transient-query/output-0.log)'
	bash -c "diff --strip-trailing-cr <(cut -d ',' -f 2- $(STEPS_DIR)/dev/expected-print.log | sort) <(cut -d ',' -f 2- $(DEV_OUTPUTS_DIR)/print-output-topic/output-0.log | sort)"
	diff --strip-trailing-cr $(STEPS_DIR)/test/expected-results.log $(TEST_OUTPUTS_DIR)/test-results.log
